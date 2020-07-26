# rpi-433-v3

Simple NodeJS module to send and receive decimal codes through 433Mhz device on RaspberryPI 2

[![npm version](https://badge.fury.io/js/rpi-433-v3.svg)](http://badge.fury.io/js/rpi-433-v3)

[![NPM](https://nodei.co/npm/rpi-433-v3.png?downloads=true)](https://nodei.co/npm/rpi-433-v3/)

## v1/v2/v3

The original library is [here](https://github.com/eroak/rpi-433/). Then [ftrier](https://github.com/ftrier/) forked it into a new repo called rpi-433-v2 which is now deleted (but still published on NPM). v2 added the ability to set a Protocol and updated the CPP to the latest. However, it seems as though v2 is broken for sniffing as the stdout for the CPP did not flush the output therefore node could not pick up the output. Therefore, I have created v3 which attempts to fix that.

### Credits

So all credits go to eroak and ftrier.

### Modifications to the original

- Protocol can be set
- Updated the scripts (codesend and RFSniffer) to the latest

### Dependencies

- wiringPi : https://projects.drogon.net/raspberry-pi/wiringpi/

### Building WiringPi

```bash
pi@raspberrypi ~ $ git clone git://git.drogon.net/wiringPi
...
pi@raspberrypi ~ $ cd wiringPi/wiringPi
pi@raspberrypi ~/wiringPi/wiringPi $ sudo su
...
root@raspberrypi:/home/pi/wiringPi/wiringPi# ./build
```

### Installation

```bash
npm install rpi-433-v3
```

### Usage

Firstly, make sure you are running your application as root or with sudo, else the Raspberry Pi will not let you output/input to the GPIO and you'll get an error.

```bash
sudo node myscript.js
```

Please note that there are different and confusing ways to reference a channel. This module supports wPi schema. Once wiringPi is installed, in your CLI you can run `gpio readall` and check the wPi column or consult https://projects.drogon.net/raspberry-pi/wiringpi/pins/

```bash
gpio readall
```

### Example

```js
var rpi433 = require("rpi-433-v3"),
  rfSniffer = rpi433.sniffer({
    pin: 2, //Sniff on GPIO 2 (or Physical PIN 13) (optional)
    pulseLength: 350,   // Listen for codes with this pulse length (optional)
    debounceDelay: 500 //Wait 500ms before reading another code
  }),
  rfEmitter = rpi433.emitter({
    pin: 0, //Send through GPIO 0 (or Physical PIN 11)
    pulseLength: 350, //Send the code with a 350 pulse length
    protocol: 4 //Set the protocol.
  });

// Receive (data is like {code: xxx, pulseLength: xxx})
rfSniffer.on("data", function(data) {
  console.log(
    "Code received: " + data.code + " pulse length : " + data.pulseLength
  );
});

// Send
rfEmitter.sendCode(1234, function(error, stdout) {
  //Send 1234
  if (!error) console.log(stdout); //Should display 1234
});

/* Or :

rfEmitter.sendCode(code);
rfEmitter.sendCode(code, {  //You can overwrite defaults options previously set (only for this sent)
  pin: 2,
  pulseLength: 350
});
rfEmitter.sendCode(code, callback);
rfEmitter.sendCode(code, {
  pin: 2,
  pulseLength: 350
}, callback);
*/

//rpi-433 uses the kriskowal's implementation of Promises so,
//if you prefer Promises, you can also use this syntax :
rfEmitter.sendCode(1234, { pin: 0 }).then(
  function(stdout) {
    console.log("Code sent: ", stdout);
  },
  function(error) {
    console.log("Code was not sent, reason: ", error);
  }
);
```
