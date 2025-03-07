import serial
import time
import mindwave  # For NeuroSky headset

# Connect to EEG headset
headset = mindwave.Headset('/dev/tty.MindWaveMobile-DevA', 9600)
time.sleep(2)  # Allow time for connection

# Connect to ESP32/Arduino via Serial
arduino = serial.Serial('COM3', 115200)  # Change COM port as needed
time.sleep(2)

print("Monitoring brainwaves...")

while True:
    attention = headset.attention  # Get attention level (0-100)
    
    if attention > 70:
        arduino.write(b'H')  # High fan speed
    elif 40 < attention <= 70:
        arduino.write(b'M')  # Medium speed
    else:
        arduino.write(b'L')  # Low speed
    
    print(f"Attention: {attention} → Fan Speed Sent")
    time.sleep(1)  # Refresh every second

