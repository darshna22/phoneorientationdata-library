# Phone Orientation Data Library

## Library Description:
This library helps to provide phone orientation data using AIDL service of interval 8ms.
Multiple applications/clients can connect to service using the SDK at a time to get the orientation data.  

## Technical Description:
*	I have used 3rd party librariey like lifecycle.

## How to use libray:
### Add it in your root build.gradle at the end of repositories:
allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}
  
### Add the dependency
dependencies {
	implementation 'com.github.darshna5:phoneorientationdata-library:1.0'
	}  


### Add service to manifiest file:
<service
  android:name="com.darshna.phoneorientationlibrary.aidl.OrientationService"
  android:enabled="true"
  android:exported="true" />

### Bind Service in Activity/Fragment:
 private fun addSensorDataObserver() {
        OrientationService.sensorData.observe(this, Observer {
            orientationText.text = it.contentToString()
        })
    }

    private fun getDataFromAIDL() {
        orientationInterface?.orientation()?.let {
            orientationText.text = it
        }
    }

    private fun bindOrientationService() {
        orientationServiceIntent = Intent(
          this,
          OrientationService::class.java
        )
        orientationServiceConnection = object :
          ServiceConnection {
            override fun onServiceConnected(componentName: ComponentName?, binder: IBinder?) {
                orientationInterface =
                    OrientationInterface.Stub.asInterface(
                      binder
                    )
                getDataFromAIDL()
            }

            override fun onServiceDisconnected(componentName: ComponentName?) {
            }
        }
        orientationServiceIntent?.let {
            orientationServiceConnection?.let {
                bindService(
                    orientationServiceIntent,
                    it,
                  Context.BIND_AUTO_CREATE
                )
            }
        }
    } 




