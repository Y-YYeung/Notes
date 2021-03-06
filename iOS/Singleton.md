# Use `Singleton` Correctly

- 创建单例

	```objc
	static ShareInstanceClass *instance = nil;
	+ (instancetype)sharedInstance {
	    	static dispatch_once_t onceToken;
	    	dispatch_once(&onceToken, ^{
	        	instance = [[[self class] alloc] init];
	    	});
	    	return instance;
	}
	```
	
	- 仅此创建，在使用 `[[ShareInstanceClass alloc] init]`、`[ShareInstanceClass new]` 时，也是会创建不同的实例

- 重写 allocWithZone:

	```objc
	+ (instancetype)allocWithZone:(struct _NSZone *)zone {
    	static dispatch_once_t onceToken;
    	dispatch_once(&onceToken, ^{
        	instance = [super allocWithZone:zone];
    	});
    	return instance;
	}
	```
	
	- 单例的其他 property 一般会在 `init` 方法中初始化，若使用者直接使用了 `init` 方法，`ShareInstanceClass` 指针确实不便，但 `ShareInstanceClass` 的 property 指针会发生改变

- 将 property 初始化或默认值防止到 `dispatch_once` 的 block 中进行

	```objc
	static ShareInstanceClass *instance = nil;
	+ (instancetype)sharedInstance {
	    	static dispatch_once_t onceToken;
	    	dispatch_once(&onceToken, ^{
	        	instance = [[[self class] alloc] init];
	        	
	        	/* Initialize the property of the ShareInstanceClass */
	        	
	    	});
	    	return instance;
	}
	```

- 较为健壮的创建代码

	```objc
	static ShareInstanceClass *instance = nil;
	+ (instancetype)sharedInstance {
			static dispatch_once_t onceToken;
	    	dispatch_once(&onceToken, ^{
	        	instance = [[[self class] alloc] init];
	        	
	        	/* Initialize the property of the ShareInstanceClass */
	        	
	    	});
	    	return instance;
	}
	
		+ (instancetype)allocWithZone:(struct _NSZone *)zone {
	    	static dispatch_once_t onceToken;
	    	dispatch_once(&onceToken, ^{
	        	instance = [super allocWithZone:zone];
	    	});
	    	return instance;
	}
	```

# Swift 中创建编写 Singleton

```swift
class Singleton  {
    static let sharedInstance = Singleton()
    private override init() {}
}
```

类变量的初始化会放在 swift_once_block_invoke 中进行，保证了唯一性

通过覆盖默认的公开初始化方法，并置为私有化，保证其他地方不能通过 init 来生成更多的实例

