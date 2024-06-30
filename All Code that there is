import kotlin.properties.ReadWriteProperty
import kotlin.reflect.KProperty

//parent class 
open class SmartDevice(val name: String, val category: String, val type: String) {
    var deviceStatus = "online"
    	protected set
    open val deviceType = "unknown"
    open fun turnOn(){
         deviceStatus = "on"
    }
    open fun turnOff(){
         deviceStatus = "off"
    }
    
    fun printDeviceInfo(){
        println("Device Name : $name | Device Category : $category | Device Type : $type | Device Status : $deviceStatus" )
    }
}
//functions for Tv
class SmartTvDevice(deviceName: String, deviceCategory : String, deviceType : String) :
		SmartDevice(name = deviceName, category = deviceCategory, type = deviceType) {
            
            override val deviceType = "Smart TV"
           
            private var speakerVolume by RangeRegulator(initialValue = 2, minValue = 0, maxValue = 100)

    	    private var channelNumber by RangeRegulator(initialValue = 1, minValue = 0, maxValue = 200) 
                       
            fun increaseSpeakerVolume(times : Int) {
                repeat (times){
                speakerVolume++ }
                println("The speaker volume has been increased to $speakerVolume.")
            }
            
            fun decreaseSpeakerVolume(times : Int) {
                repeat (times){
                speakerVolume-- }
                println("The speaker volume has been decreased to $speakerVolume.")
            }
            
            fun nextChannel(times : Int){
                repeat (times){
                channelNumber++ }
                println("The channel number has been set to $channelNumber .")
            }
            
            fun previousChannel(times : Int){
                repeat (times) {
                channelNumber-- }
                println("The channel number has been set to $channelNumber")
            }
            
            override fun turnOn() {
        		super.turnOn()
        		println("$name is turned on."+ 
                        "Speaker volume is set to $speakerVolume and channel number is set to $channelNumber.")
            }
            
            override fun turnOff() {
        		super.turnOff()
        		println("$name turned off")
    		}
          	
            fun printTvInfo(){
                super.printDeviceInfo()
            }
        }
//Functions for light        
class SmartLightDevice(deviceName:String, deviceCategory:String, deviceType : String) :
		   SmartDevice(name = deviceName , category = deviceCategory, type = deviceType) {
               
            override val deviceType = "Smart Light"
           	private var brightnessLevel by RangeRegulator(initialValue = 0, minValue = 0, maxValue = 100)    
                         
            fun increaseBrightness(times : Int){
                repeat (times){
                brightnessLevel++ }
                println("The brightness level has been increased to $brightnessLevel.")
            }
            
            fun decreaseBrightness(times : Int){
                repeat (times){
                brightnessLevel-- }
                println("The brightness level has been decreased to $brightnessLevel.")
            }
            
            override fun turnOn() {
    			super.turnOn()
        		brightnessLevel = 2
        		println("$name turned on. The brightness level is $brightnessLevel.")
        		
            }
            
            override fun turnOff() {
        		super.turnOff()
        		brightnessLevel = 0
        		println("$name turned off")	
    		}
            
            fun printLightInfo(){
                super.printDeviceInfo()
            }
        }
//Class made by ChatGPT, checks device.status to run commands, making sure there are no bugs       
class DeviceController(private val device: SmartDevice) {

    private fun canOperate(): Boolean = device.deviceStatus in listOf("on", "online", "off")

    fun turnOn(action: () -> Unit) {
        if (canOperate() && device.deviceStatus != "on") {
            action()
        } else if (device.deviceStatus == "on") {
            println("Cannot perform action. ${device.name} is already on.")
        } else { 
            println("Cannot perform action. $device.name is not operational")
        }
        
    }

    fun turnOff(action: () -> Unit) {
        if (canOperate() && device.deviceStatus == "on") {
            action()
        } else if (device.deviceStatus == "off") {
            println("Cannot perform action. ${device.name} is already off.")
        } else {println("Cannot perform action. $device.name is not operational")}
    }

    fun performIfOn(action: () -> Unit) {
        if (device.deviceStatus == "on") {
            action()
        } else {
            println("Cannot perform action. ${device.name} is not turned on.")
        }
    }
}
        
        
        
//To control all devices and functions  
class SmartHome(
    val smartTvDevice : SmartTvDevice,
    val smartLightDevice : SmartLightDevice
) {
    public var deviceTurnOnCount = 0
    	private set

    private val tvController = DeviceController(smartTvDevice)
    private val lightController = DeviceController(smartLightDevice)//
    
    
    
    fun turnOnTv() {
        tvController.turnOn{
        	deviceTurnOnCount++
        	smartTvDevice.turnOn()
    	}
    }
    
    fun turnOffTv() {
        tvController.turnOff {
        deviceTurnOnCount--
        smartTvDevice.turnOff() }
    }
    
   	fun increaseTvVolume(times : Int) {
        tvController.performIfOn {
        smartTvDevice.increaseSpeakerVolume(times)}
    }

    fun decreaseTvVolume(times : Int) {
        tvController.performIfOn{
        smartTvDevice.decreaseSpeakerVolume(times)}
    }

    fun changeTvChannelToNext (times : Int) {
        tvController.performIfOn{
        smartTvDevice.nextChannel(times)}
    }

    fun changeTvChannelToPrevious(times : Int) {
        tvController.performIfOn{
        smartTvDevice.previousChannel(times)}
    }
    
    fun turnOnLight() {
        lightController.turnOn{
        deviceTurnOnCount++
        smartLightDevice.turnOn()}
    }
    
    fun turnOffLight() {
        lightController.turnOff{
        deviceTurnOnCount--
        smartLightDevice.turnOff()}
    }
    
    fun increaseLightBrightness(times : Int) {
        lightController.performIfOn{
        smartLightDevice.increaseBrightness(times)}
    }

    fun decreaseLightBrightness(times : Int) {
        lightController.performIfOn{
        smartLightDevice.decreaseBrightness(times)}
    }
    
    fun turnOffAllDevices() {
        if (smartTvDevice.deviceStatus == "on" && smartLightDevice.deviceStatus == "on"){ 
        turnOffTv()
        turnOffLight()
        println("All devices turned off")
        } else if (smartTvDevice.deviceStatus == "on") {
            turnOffTv()
            println("Smart Light was already off.")
        } else if (smartLightDevice.deviceStatus ==  "on") {
            turnOffLight()
            println("Smart Tv was already off.")
        } else {println("No device currently on.")}
    }

    fun LightInfo() {
        smartLightDevice.printLightInfo()
    }
    
    fun TvInfo(){
        smartTvDevice.printTvInfo()
    }
    
    
    fun seeDevices(){
        println("$deviceTurnOnCount devices are on.")
    }    
} 

//To shorten the get and set functions.
class RangeRegulator(initialValue: Int,
    				 private val minValue: Int,
    			     private val maxValue: Int)
		: ReadWriteProperty<Any?, Int> {
            
    var fieldData = initialValue
            
    override fun getValue (thisRef: Any?, property : KProperty<*>) : Int {
        return fieldData
    }
    
    override fun setValue (thisRef: Any?, property: KProperty<*>, value : Int){
         if (value in minValue..maxValue) {
            fieldData = value
         }   
    }
    
    
}
        
fun main() {
    //To control with limited functions
    var smartDevice: SmartDevice = SmartTvDevice("Android TV", "Entertainment", "SmartTV")
    //add commands here for Tv    
    smartDevice = SmartLightDevice("Google Light", "Utility", "Smart Light")
    //add commands here	for Light
    println("")
    //To control with large amount of functions
    val nameOfTv : String = "Android TV"
    val nameOfLight : String = "Google Light"
    val smartHome = SmartHome(SmartTvDevice(nameOfTv, "Entertainment", "SmartTV"),
                              SmartLightDevice(nameOfLight, "Utility", "Smart Light"))
    //add commands here for both
    
}
