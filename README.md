# BlindSideDuino #
Basic Schematic and Codes :: Pet's collar for visually challenged / blind pets


## Code ##


```

int SPEED = 0;

void setup() {
  // Mini UltraSonic Sensor
  pinMode(2, OUTPUT); // PingSensor
  pinMode(4, INPUT);  // Ping Return

  // LEDs
  pinMode(5, OUTPUT); // Dist 0
  pinMode(6, OUTPUT); // Dist 1
  pinMode(7, OUTPUT); // Dist 2
  pinMode(8, OUTPUT); // Dist 3

  // Notifier 
  // (In my Case I am using a simple pc speaker)
  pinMode(10, OUTPUT);
  
  // Serial Printing
  // For debugging
  Serial.begin(9600);
}



void loop() {
  Serial.print("\n[START_LOOP]\n");
  // Delay Startup:
  //  - For Nice Debugginh header :P
  delay(10);
  
  digitalWrite(5, LOW);
  digitalWrite(6, LOW);
  digitalWrite(7, LOW);
  digitalWrite(8, LOW);

  digitalWrite(10, LOW);

  Serial.print("\n[CLEARED]\n");
  Serial.print("\n[SENDING_PING]\n");


  // put your main code here, to run repeatedly:
  digitalWrite(2, LOW);
  delayMicroseconds(4);
  digitalWrite(2, HIGH);
  delayMicroseconds(10);
  digitalWrite(2, LOW);

  long t = pulseIn(4, HIGH);


  // I don't understand feet/inches very well...
  //long inches = t / 74 / 2;
  long cm = t / 29 / 2;
  //String inch = " inches \t";
  String CM = "cm ";

  Serial.print(cm + CM);
  Serial.print("\n------\n");

  // If pet is laying down:
  //    - Don't notify
  if (cm == 0 ){
    digitalWrite(5, LOW);
    digitalWrite(6, LOW);
    digitalWrite(7, LOW);
    digitalWrite(8, LOW);
    digitalWrite(10, LOW);
    return;
  }

  // I'm assuming:
  // - Average large dog has about 20 cm-ish,
  // - Dist between collar and food bowl
  if (cm <= 20){
    delay(15);
    return;
  }

  // This should be quite close to pets' snout
  else if (cm <= 50){
    digitalWrite(8, HIGH);
    viben(6);
    return;
  }

  // A bit close to object...
  else if (cm <= 75){
    digitalWrite(7, HIGH);
    viben(3);
    return;
  }

  // Safe distance...
  else if (cm <= 140){
    digitalWrite(6, HIGH);
    viben(1);
    return;
  }

  // Far enough, pet doesn't need to worry..
  else if (cm >= 750 ){
    digitalWrite(5, HIGH);
    return;
  }

}




// Was originally intended to use:
// - 'off balanced' rotor.. 
// - but wasn't working right..
void viben(int SPEED){
  int i = 0;
  while (i <= 3){
    digitalWrite(10, HIGH);
    delay(150 / SPEED);
    Serial.println("[Vibes/Beeps]: ");
    Serial.println(i);
    digitalWrite(10, LOW);
    delay(150);
    i = i + 1;
  }
  return;
}


```



## Basic Diagram ##

Key:
<br>
(X-#)     - LED
<br>
(GND)     - Leads to Ground/Negative/Nutral
<br>
(TX)      - Echo/OutPut/SCL/Return
<br>
(RX)      - Trigger/InPut/SDA
<br>
(VCC)     - Voltage in -> 5 Volt <-
<br>



```

       ___________________________________________
      |        |        |        |         |
(X-0) || (X-1) || (X-2) || (X-3) ||         -> (GND)
       |        |        |        |
   pin(5)   pin(6)   pin(7)   pin(8)
                 
                      
                       (Micro Utra Sonic Sensor)                
    (PWR)             ----------------------------
.--------------.      :          ___             :
|              |      :        /     \           :
|              |      :       (  ()   )          :
|              |      :        \_____/           :== -- (VCC) -> pin(24)
|              |      :                          :
|              |      :                          :== -- (RX)  -> pin(2)
|              |      :          ___             :
|              |      :        /     \           :== -- (TX)  -> pin(4)
|              |      :       (  ()   )          :
|              |      :        \_____/           :== -- (GND) -> pin(3) 
|              |      :                          :            ^ {Cause I use it for debugging}
|              |      :                          :
|              |      :                          :
 \-----||------/      ----------------------------
      /  \
 (GND)    (VCC)=>(5-volt-Regulator)
 
 
 (Arduino Nano -Old Module)
 --|14|--|13|--|12|--|11|--|10|--|09|--|08|--|07|--|06|--|05|--|04|--|03|--|02|--|01|--|00|--
|                                                                                             |
|                                                                                             |
|  [.] [.]                                                                                   [ ]
|  [.] [.]                                                                                   | |
|  [.] [.]                                                                                   [ ]
|                                                                                             |
|                                                                                             |
 --|15|--|16|--|17|--|17|--|18|--|19|--|20|--|21|--|22|--|23|--|24|--|25|--|26|--|27|--|28|--


```



## To Follow ##

If all goes well, I will be adding an esp-01 to the project.
This should allow modification to be made:
<br>
-> Connect via app
<br>
-> Adding different voicenotes for difference distances
<br>
-> Adding 'sleep mode timer' 
<br>
-> Adjusting distance segments
<br>
-> Chat with your pet
<br>
-> Change colors of LEDs
<br>
-> -> much more, but my ADHD will go nuts here...







