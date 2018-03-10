# Python

Examples for using the PLAY BOARD with Python

## Components

### LED

Hello world:

```python
from gpiozero import LED

led = LED(10)

led.blink()
```

More:

```python
led.on()
led.off()
led.blink(2, 2)
led.blink(0.1, 0.1)
```

### Buzzer

Hello world:

```python
from gpiozero import Buzzer

buzz = LED(10)

buzz.beep()
```

More:

```python
buzz.on()
buzz.off()
buzz.beep(2, 2)
buzz.beep(0.1, 0.1)
```

### Button

Hello world:

```python
from gpiozero import Button

button = Button(10)

while True:
    if button.is_pressed:
        print("Pressed")
    else:
        print("Not pressed")
```

More:

```python
button.when_pressed = function
button.when_released = function
button.when_held = function
```

### PWM LED

Hello world:

```python
from gpiozero import PWMLED

led = PWMLED(10)

led.pulse()
```

More:

```python
led.on()
led.off()
led.value = 0.5
led.blink(2, 2)
led.blink(0.1, 0.1)
led.pulse(2, 2)
led.pulse(0.1, 0.1)
```

## Recipes

### Traffic Lights

Code a traffic lights sequence:

```python
from gpiozero import LED
from time import sleep

red = LED(10)
amber = LED(11)
green = LED(12)

while True:
    green.on()
    amber.off()
    red.off()
    sleep(10)
    green.off()
    amber.on()
    sleep(1)
    amber.off()
    red.on()
    sleep(10)
    amber.on()
    sleep(1)
```

### Interactive Traffic Lights

Code an interactive traffic lights sequence:

```python
from gpiozero import LED
from time import sleep

red = LED(10)
amber = LED(11)
green = LED(12)
button = Button(13)

while True:
    green.on()
    amber.off()
    red.off()
    button.wait_for_press()
    sleep(5)
    green.off()
    amber.on()
    sleep(1)
    amber.off()
    red.on()
    sleep(10)
    amber.on()
    sleep(1)
```

### LED + Button

Press the button to light the LED:

```python
from gpiozero import LED, Button

led = LED(10)
button = Button(11)

while True:
    if button.is_pressed:
        led.on()
    else:
        led.off()
```

or:

```python
from gpiozero import LED, Button

led = LED(10)
button = Button(11)

button.when_pressed = led.on
button.when_released = led.off
```

### Buzzer + Button

Press the button to buzz the Buzzer:

```python
from gpiozero import LED, Buzzer

led = LED(10)
buzz = Buzzer(11)

while True:
    if button.is_pressed:
        buzz.on()
    else:
        buzz.off()
```

or:

```python
from gpiozero import LED, Buzzer

buzz = LED(10)
button = Button(11)

button.when_pressed = buzz.on
button.when_released = buzz.off
```

### Reaction game

When you see the light come on, the first person to press their button wins!

```python
from gpiozero import Button, LED
from time import sleep
import random

led = LED(10)

player_1 = Button(11)
player_2 = Button(12)

time = random.uniform(5, 10)
sleep(time)
led.on()

while True:
    if player_1.is_pressed:
        print("Player 1 wins!")
        break
    if player_2.is_pressed:
        print("Player 2 wins!")
        break

led.off()
```

### Button controlled camera

Connect a camera module and have the button take a picture:

```python
from gpiozero import Button
from picamera import PiCamera
from datetime import datetime

button = Button(10)
camera = PiCamera()

def capture():
    datetime = datetime.now().isoformat()
    camera.capture('/home/pi/{}.jpg'.format(datetime))

button.when_pressed = capture
```

### GPIO Music Box

Each button plays a different sound!

```python
from gpiozero import Button
import pygame.mixer
from pygame.mixer import Sound

pygame.mixer.init()

button_sounds = {
    Button(10): Sound("drum.wav"),
    Button(11): Sound("cymbal.wav"),
    Button(12): Sound("snare.wav"),
    Button(13): Sound("tom.wav"),
}

for button, sound in button_sounds.items():
    button.when_pressed = sound.play
```
