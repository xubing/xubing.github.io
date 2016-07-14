---
layout:  post
title:  Local Notification App的本地通知 iOS&Android
tags: Notification iOS Android
description: Local Notification App的本地通知 iOS&Android
comments: true
---


### Local Notification 


### iOS UILocalNotification 

这个ios的本地通知在框架UIKit中，可以子在iOS 4.0 and later中使用。本地通知可以按照设定的时间进行中展现，系统在计划的时间点进行分发本地通知。虽然类似远程通知，比如播放声音，在icon上显示数字，但是她不用连接远程服务，完全本地奋发的。
	
本地通知的主要目的是基于时间的行为。一个app有有限数目的本地通知，系统保持最近的64个同事是活跃的，自动计划。
	
  当你创建一个本地通知的时候，你你必须指明特定的日期或者地理区域作为分发通知的触发条件。基于时间的通知可以根据你每天的设置，如果需要，也允许改变时区。基于地理区域的通知是当用户进入或者退出区域的时候进行处罚。通知可以是一次性事件，也可以是每天有计划执行。
	
创建一个UILocalNotification对象后，相关操作函数
	
```
scheduleLocalNotification:
presentLocalNotificationNow:.
cancelLocalNotification:
cancelAllLocalNotifications 	 
```
	
### 事件
	
	系统分发本地消息的时候，有几种情况对待。app不在展示或不可见的时候，如果用户点击按钮，会激活app进行登录。
	 In its application:didFinishLaunchingWithOptions: method, the app delegate can obtain the UILocalNotification object from the launch options dictionary using the **UIApplicationLaunchOptionsLocalNotificationKey** key. 
	 
	 When the user selects a custom action, the app delegate’s application:handleActionWithIdentifier:forLocalNotification:completionHandler: method is called to handle the action.
	 
	 当用户在前端而且可见的时候， the app delegate’s application:didReceiveLocalNotification: is called to process the notification. 系统不会展现任何的警告，dadge ，也不会播放声音。
	 
### 代码编写

* 创建
{% highlight C++ linenos %}
UILocalNotification *notification = [[UILocalNotification alloc] init];
        // 设置触发通知的时间
    NSDate *fireDate = [NSDate dateWithTimeIntervalSinceNow:secondes];
    NSLog(@"fireDate=%@",fireDate);
    
    notification.fireDate = fireDate;
        // 时区
    notification.timeZone = [NSTimeZone defaultTimeZone];
        // 设置重复的间隔
    notification.repeatInterval = 0;
    
    // 通知内容
    notification.alertBody =  body;
    notification.applicationIconBadgeNumber = 1;
        // 通知被触发时播放的声音
    notification.soundName = UILocalNotificationDefaultSoundName;
        // 通知参数
    NSDictionary *userDict =@{@"body":body,key:@"1"};
//    [NSDictionary dictionaryWithObject:body forKey:@"body"];
//    [userDict setValue:@"1" forKey:key];
    notification.userInfo = userDict;
    notification.repeatInterval = 0;
    
        // ios8后，需要添加这个注册，才能得到授权
    if ([[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        UIUserNotificationType type =  UIUserNotificationTypeAlert | UIUserNotificationTypeBadge | UIUserNotificationTypeSound;
        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:type
                                                                                 categories:nil];
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            // 通知重复提示的单位，可以是天、周、月
        notification.repeatInterval = 0;
    } else {
            // 通知重复提示的单位，可以是天、周、月
        notification.repeatInterval = 0;
    }  
    
        // 执行通知注册  
    [[UIApplication sharedApplication] scheduleLocalNotification:notification];	 
{% endhighlight C++ %}

* 取消
{% highlight C++ linenos %}
    // 获取所有本地通知数组
    NSString* key = [dict objectForKey:@"key"];
    NSArray *localNotifications = [UIApplication sharedApplication].scheduledLocalNotifications;
    
    for (UILocalNotification *notification in localNotifications) {
        NSDictionary *userInfo = notification.userInfo;
        if (userInfo) {
                // 根据设置通知参数时指定的key来获取通知参数
            NSString *info = userInfo[key];
            
                // 如果找到需要取消的通知，则取消
            if (info != nil) {
                [[UIApplication sharedApplication] cancelLocalNotification:notification];
                break;  
            }  
        }  
    }  
    
    
    	 
{% endhighlight C++ %}


 * 更新显示的徽章个数

{% highlight C++ linenos %}
    NSInteger badge = [UIApplication sharedApplication].applicationIconBadgeNumber;
    badge--;
    badge = badge >= 0 ? badge : 0;
    [UIApplication sharedApplication].applicationIconBadgeNumber = badge;
{% endhighlight C++%}


### Android的本地通知

* 可以用AlarmManager也可以用Service。而且还有一个子类IntentService更好。根据需求进行判断使用。
* 创建一个Service
	
	startService调用后，服务启动，会走一次onCreate()，	onStartCommand,你再次调用startService后，就只走 	onStartCommand。可以根据	onStartCommand进行设置删除app的时候服务器的restart情况。有三种状态，其中START_REDELIVER_INTENT是重新分发以前的Intent。更高级的，比如service之间的通讯等，因为不需要，没有继续。

	
	{% highlight java linenos%}
	
	public class NotificationService extends Service {

	public static final String tag = "BingLog";	
	private Handler m_handler = null;
	private Runnable[] m_runnable = null;
	
	@Override
	public void onCreate(){		
		super.onCreate();
		Log.d(tag, "service oncreate");
	}
	
	@Override
	public int onStartCommand(Intent intent ,int flags,int startId){
		Log.d(tag, "service onStartCommand");
		
		int count =  (intent.getExtras().getInt("count"));		
		Log.d(tag, "notification service canceled");
		if (m_runnable != null)
		{
			for(int index = 0;index < m_runnable.length; ++ index)
			{
				m_handler.removeCallbacks(m_runnable[index]);
			}
		}
					
		m_runnable = new Runnable[2];
		for(int index = 0; index<count ; ++index)
		{
			String tmpkey = "";
			long secondes = 0;
			if (intent == null)
			{
				Log.d(tag, "onStartCommand intent is null");
				tmpkey = "demo";
				secondes = 10;
			}
			else
			{
				Log.d(tag,"body" + String.valueOf(index+1));
				Log.d(tag,"seconds" + String.valueOf(index+1));
				tmpkey = (String) intent.getExtras().get("body" + String.valueOf(index+1));
				secondes = (intent.getExtras().getLong("seconds" + String.valueOf(index+1)));						
			}
			Log.d(tag,tmpkey);
//			tmpkey = (String) intent.getExtras().get("key");
			final String key = tmpkey;
			
			m_handler = new Handler();			
			Runnable runnable = new Runnable(){
				@SuppressWarnings("deprecation")
				@Override
				public void run() {									
				//获取到通知管理器
					NotificationManager mNotificationManager=(NotificationManager)getSystemService(Context.NOTIFICATION_SERVICE);
					
					//定义内容
					int notificationIcon= R.drawable.icon;
					CharSequence notificationTitle="Bing Demo";
					long when = System.currentTimeMillis();			
					Notification notification = new Notification(notificationIcon, notificationTitle, when);
					
					notification.defaults=Notification.DEFAULT_ALL;
					
					Intent intent2= new Intent( getApplicationContext(),PfpokerActivity.class);
					PendingIntent pendingIntent=PendingIntent.getActivity(getApplicationContext(), 0, intent2, 0);
					notification.setLatestEventInfo(getApplicationContext(),"Bing Title", key,pendingIntent);
									
					mNotificationManager.notify(100, notification);
				  }						
			};		
			m_handler.postDelayed( runnable, 1000*secondes);		
			m_runnable[index] = runnable;
		}
			
		return START_REDELIVER_INTENT;//super.onStartCommand(intent, flags, startId);
	}
	
	@Override
	public void onDestroy(){
		if ((m_handler != null )&&(m_runnable != null))
		{
			Log.d(tag, "notification service canceled");
			int count = m_runnable.length;
			for(int index = 0;index < count; ++ index)
			{
				m_handler.removeCallbacks(m_runnable[index]);
			}				
		}
		Log.d(tag, "notification service onDestroy");
		super.onDestroy();		
	}
	
	@Override
	public boolean onUnbind(Intent intent) {
	    // All clients have unbound with unbindService()
	    return false;
	}

	
	@Override
	public IBinder onBind(Intent arg0) {
		// TODO Auto-generated method stub
		return null;
	}
}

	{% endhighlight java %}
	
* 创建Notification的代码如上，可以设置声音图标标题，内容。

* 启动服务 

{% highlight java linenos%}
	Intent startintent = new Intent(s_instance,NotificationService.class);
	//添加一个额外数据
	startintent.putExtra("count",count);
	startintent.putExtra("body1",body1);        	startintent.putExtra("seconds1",(long)seconds1);
	startService(startintent);
{% endhighlight java %}

* 终止服务

{% highlight java linenos%}
	Intent startintent1 = new Intent(s_instance,NotificationService.class);        	stopService(startintent1);
{% endhighlight java %}  

* 需要在**AndroidManifest.xml**中添加一个service服务，否则不启动的。我开始坑了我半天才发现。
* 
{% highlight xml linenos%}
<service android:name = "name" android:process=":xxnmae"></service>
{% endhighlight xml %}  
