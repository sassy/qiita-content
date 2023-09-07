---
title: '[android-ndk]NativeActivityからJavaのメソッドを呼ぶ方法'
tags:
  - Android
private: false
updated_at: '2014-01-31T18:16:39+09:00'
id: 6363aeaca76fd7eba23f
organization_url_name: null
slide: false
---
* NativeActivityを使ったときに、JavaのAPIを呼ぶ方法をまとめる。
* 試したndkのバージョンはandroid-ndk-r9b

#  必要なもの
* JavaVM *vm
* JNIEnv *env

JavaVMとJNIEnvはandroid_main()に渡される struct android_app が持っているANativeActivityのメンバからとってくる

```cpp
JNIEnv *env = state->activity->env;
JavaVM *vm = state->activity->vm;
```

# Javaメソッドを呼び出す方法
* 現在のスレッドをVMにアタッチする
* クラスを取得する
* クラスからmethod idを取得する
* クラス/オブジェクトとmethod idからメソッドを呼び出す
* 現在のスレッドをVMからデタッチする

```cpp:Log.dを呼び出す
    vm->AttachCurrentThread(&env, NULL);

    jclass log = env->FindClass("android/util/Log");
    if (log == NULL) {
        LOGE("Log class not Found");
        return;
    }

    jmethodID methodD = env->GetStaticMethodID(log, "d", "(Ljava/lang/String;Ljava/lang/String;)I");
    if (methodD == NULL) {
        LOGE("Log.d not Found");
        return;
    }

    jstring tag =  env->NewStringUTF("log");
    jstring message =  env->NewStringUTF("message");

    env->CallStaticIntMethod(log, methodD, tag, message);
```

上記はサンプルのため、android/util/Log を呼び出してみたもの(実際は__android_log_printを使った方がよい。

# NativeActivityを拡張したクラスに定義したメソッドを呼び出す方法

Activityにメソッドを定義

```java:MyTest.java
public class MyTest extends NativeActivity {

    public void showToast() {
    	runOnUiThread(new Runnable() {
			@Override
			public void run() {
				Toast.makeText(MyTest.this, "test toast", Toast.LENGTH_SHORT).show();	
			}
    	});
    }
}
```

下記の用にshowToastメソッドを呼び出す。
Activityのオブジェクトは、struct android_appのNativeActivityが持っているclazzを利用します。

```cpp:Activityのメソッドを呼び出す
    vm->AttachCurrentThread(&env, NULL);

    jclass test = env->GetObjectClass(clazz);
    jmethodID methodShow = env->GetMethodID(test, "showToast", "()V");
    if (methodShow == NULL) {
        LOGE("showToast not Found");
        return;
    }
    jobject clazz = state->activity->class;
    env->CallVoidMethod(clazz, methodShow);

    vm->DetachCurrentThread();
```

# GetMethodIDに渡すメソッドのシグネチャ
* GetMethodIDは env->GetMethodID(クラス, メソッド名, メソッドのシグネチャ) で指定する
* void foo() は "()V" (戻り値がVoidも場合はV)
* int bar(int) は "(I)I" (intはIを指定)
* String baz (String str) は "(Ljava/lang/String;)Ljava/lang/String;"  (L(パケーッジ名含むクラス名); という形式で渡す)
