---
layout: article
title:  android studio gradle 发布应用
categories: archives
image:
---

android studio 的使用便捷性，我想大家都是深有体会的，最近我也在研究as，希望在这里记录下我研究as的过程，同时和大家交流和分享在这个过程中遇到的问题。

我第一次学习studio的gradle发布程序是从技术大牛stormzhang学习的，这是他的博客<http://stormzhang.com/android/2014/07/07/learn-android-from-rookie/>，这里面有详细的记录gradle的基础和使用，更重要的是介绍了多渠道打包。是一个初学者很容易看懂的博客，stormzhang的博客也是很接地气，是我见过很多博客里比较推荐的博客。好的回归正题。

  在stormzhang的博客里我们能学到gradle的大部分的应用，而我在使用中有更高的需求，我想把包打到自己指定的目录下面，找了各种资料，最后得到了一些经验，贡献给大家，希望大家能学习点东西。
  
  step 1
  
  在项目的目录下（build.gradle的根目录下面）建立gradle.properties文件，此文件的里写入配置文件
  
	STORE_FILE=/Users/xxxx/Documents/keystore/cc.mind.keystore
    STORE_PASSWORD=xxxx
    KEY_ALIAS=xxxx
    KEY_PASSWORD=xxxx
    OUTPUT_DIR=/Users/xxx/Documents/workspace1/SquareDance1/SquareDance/build/


一些配置属性可以放在此文件里，然后在使用中build.gradle里直接使用关键字就可以了，不需要任何的引用。大家注意我写的输出文件名称 OUTPUT_DIR就是我配置的输出文件的名称，然后我我在build.gradle里的使用情况如下：


	apply plugin: 'com.android.application'

	android {
    	compileSdkVersion 21
    	buildToolsVersion "21.1.2"
    	defaultConfig {
        	applicationId "com.bokecc.dance"
        	minSdkVersion 9
        	targetSdkVersion 21
        	versionCode 4
        	versionName "1.0.3"

        	//dex突破65535的限制
        	multiDexEnabled true
        	//默认是guanfang的渠道
        	manifestPlaceholders = [UMENG_CHANNEL_VALUE: "wandoujia"]

    	}

    	//
    	lintOptions {
        	abortOnError false
    	}

    	//签名
    	signingConfigs {
        	debug {
            	// no debug config
        	}

        release {

            storeFile     file(STORE_FILE)
            storePassword STORE_PASSWORD
            keyAlias      KEY_ALIAS
            keyPassword   KEY_PASSWORD
        }
    }


    //打包
    buildTypes {
        	debug {
            	//显示log
            	buildConfigField "boolean", "LOG_DEBUG", "true"

            	versionNameSuffix "-debug"
            	minifyEnabled false
            	//压缩优化
            	zipAlignEnabled false
            	//移除无用的资源文件
            	shrinkResources false
            	signingConfig signingConfigs.debug

        	}
        	release {
            	//不显示log
            	buildConfigField "boolean", "LOG_DEBUG", "false"
            	//是否混淆
            	minifyEnabled true
            	//压缩包优化
            	zipAlignEnabled true
            	//移除无用的resource文件
            	shrinkResources true

          		proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-project.txt'

            	//apk签名
            	signingConfig signingConfigs.release

            	// this is used to alter output directory and file name. If you don't need it
            	// you can safely comment it out.
            	applicationVariants.all { variant ->

                	variant.outputs.each { output ->

                    	String temp = (String)OUTPUT_DIR + defaultConfig.versionName
                    	if (project.hasProperty('OUTPUT_DIR') && new File(temp).exists()){
                    	}else{
                       		new File(temp).mkdir();
                    	}

                    	String parent = temp

                    	//输出apk名称格式为SquareDance_v1.0_4_wandoujia.apk
                    	def fileName = "SquareDance_v${defaultConfig.versionName}_${defaultConfig.versionCode}_${variant.productFlavors[0].name}.apk"

                   		output.outputFile = new File(parent, fileName)

                	}
            	}
            // end your comment here

        	}
    	}

    	//多渠道包设置
    	productFlavors {
        	xiaomi {}
        	_360 {}
        	baidu {}
        	wandoujia {}
        	guanfang {}
        	_91 {}
        	anzhuo {}
        	huawei {}
    	}

    	productFlavors.all {
        	flavor -> flavor.manifestPlaceholders = [UMENG_CHANNEL_VALUE: name]
    	}

	}

	dependencies {
    	compile project(':libCropper')
    	compile project(':libPulltorefreshDa')
    	compile project(':libSMSSDK')
    	compile project(':libViewPagerIndicator')
    	compile 'com.google.code.gson:gson:2.3.1'
    	compile 'com.android.support:support-v4:22.0.0'
    	compile files('libs/CCSDK.jar')
    	compile files('libs/Xg_sdk_v2.37.jar')
    	compile files('libs/httpmime-4.2.5.jar')
    	compile files('libs/libammsdk.jar')
    	compile files('libs/libidn-1.28.jar')
    	compile files('libs/mid-sdk-2.10.jar')
    	compile files('libs/mta-sdk-1.6.2.jar')
    	compile files('libs/nineoldandroids-library-2.4.0.jar')
    	compile files('libs/open_sdk.jar')
    	compile files('libs/picasso-2.3.4.jar')
    	compile files('libs/umeng_sdk.jar')
    	compile files('libs/weibosdkcore.jar')
    	compile files('libs/wup-1.0.0-SNAPSHOT.jar')
	}

  
  大家觉得有用，尽管拿去。