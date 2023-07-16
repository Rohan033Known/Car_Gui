# Car_Gui
The GUI-controlled car project is an exciting venture that combines the power of Arduino microcontrollers and graphical user interfaces (GUI) to control a small car wirelessly.
#please don't follow circuit diagram of ppt & pdf ....




from pyfirmata import Arduino
import time
import threading
import tkinter 
import customtkinter
from pynput.keyboard import Key , Listener

#arduino connection

board = Arduino('COM5')


pin_3 = board.get_pin('d:3:p')
pin_6 = board.get_pin('d:6:p')
pin_2 = board.get_pin('d:2:o')
pin_4 = board.get_pin('d:4:o')
pin_5 = board.get_pin('d:5:o')
pin_7 = board.get_pin('d:7:o')

speed = 0.6
Value = " "
Value2 = " "


def forward():
    global speed
    pin_3.write(speed)
    pin_5.write(0)
    pin_7.write(1)
    pin_6.write(speed)
    pin_2.write(0)
    pin_4.write(1)

def stop():
    pin_3.write(0)
    pin_5.write(0)
    pin_7.write(0)
    pin_6.write(0)
    pin_2.write(0)
    pin_4.write(0)

def reverse():
    global speed
    pin_6.write(speed)
    pin_2.write(1)
    pin_4.write(0)
    pin_3.write(speed)
    pin_5.write(1)
    pin_7.write(0)

def turn_right():
    global speed
    pin_6.write(speed)
    pin_2.write(1)
    pin_4.write(0)
    pin_3.write(speed)
    pin_5.write(0)
    pin_7.write(1)

def turn_left():
    global speed
    pin_6.write(speed)
    pin_2.write(0)
    pin_4.write(1)
    pin_3.write(speed)
    pin_5.write(1)
    pin_7.write(0)

def start_car():
    app.destroy()

def exit():
    app.destroy()

#gui design

customtkinter.set_appearance_mode('System')
customtkinter.set_default_color_theme('blue')

app= customtkinter.CTk()
app.geometry("700x450")
app.title("Remote Gui")

titel= customtkinter.CTkLabel(app,text="Control Section")
titel.pack(padx=10, pady=10)

button1 = customtkinter.CTkButton(app,text="Start" , command= start_car  )
button1.pack(padx=2, pady=2)
button7 = customtkinter.CTkButton(app, text="Stop" , command= stop)
button7.pack()
button6 = customtkinter.CTkButton(app, text="Exit" , command= exit)
button6.pack( padx=20, pady=20)

button2 = customtkinter.CTkButton(app, text="Forward" , command= forward)
button2.pack(padx=50, pady=30)

button4 = customtkinter.CTkButton(app, text="Left" , command= turn_left)
button4.pack(side=customtkinter.LEFT, padx=50, pady=10)

button5 = customtkinter.CTkButton(app, text="Right" , command= turn_right)
button5.pack(side=customtkinter.RIGHT, padx=50, pady=10)

button3 = customtkinter.CTkButton(app, text="Reverse" , command= reverse)
button3.pack(side=customtkinter.BOTTOM,padx=20, pady=20)


def on_pass(key):
    global Value
    global Value2
    try:
        if key.char =='w':
            Value =1
            forward()
        elif key.char == 'a':
            turn_left()
           
        elif key.char=='d' :
                turn_right()
        elif key.char == 's':
                Value2 = 1
                reverse()
        else:
                stop()
    except :
        stop()

def on_release(key):
    global Value
    global Value2
    try :
        if key.char =='w':
            Value = 0
            stop()
        if key.char == 's':
            Value2 = 0
            stop()
        elif key.char =='a' or key.char=='d':
            if Value == 1:
                forward()
            elif Value2 ==1:
                reverse()
            else :
                stop()
        else:
            stop()
    except:
        stop()

def get_key():
    with Listener(
        on_press=on_pass, on_release=on_release) as Listener:
        Listener.Join()
thread1=threading.Thread(target=get_key)

app.mainloop()
