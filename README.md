# liuyipeng11
import RPi.GPIO as GPIO import time import smtplib
from email.message import EmailMessage from datetime import datetime
# Configuration
SENSOR_CHANNEL = 4	# GPIO4
SENDER_EMAIL = "31143619129@qq.com"
SENDER_PASSWORD = "123"
RECIPIENT_EMAIL = "rslinnnn@gmail.com"
SMTP_SERVER = "smtp.office365.com"
SMTP_PORT = 587
CHECK_INTERVAL = 21600	# 6 hours (4x daily)
# Initialize GPIO
GPIO.setmode(GPIO.BCM)
GPIO.setup(SENSOR_CHANNEL, GPIO.IN)
# Email function (reusable) def send_alert(status):
"""Sends formatted email based on soil status."""
	timestamp	=
datetime.now().strftime("%Y-%m-%d %H:%M:%S") subject = f"Plant Alert: {status}" body = f"""\
Plant Status Update ({timestamp})
-
Soil moisture level: {status}
Action required: {"Water the plant!" if "DRY" in status else "No action needed."}
"""
msg = EmailMessage() msg.set_content(body) msg['Subject'] = subject
msg['From'] = SENDER_EMAIL msg['To'] = RECIPIENT_EMAIL
try:
with smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
as server:
server.starttls()
server.login(SENDER_EMAIL,
SENDER_PASSWORD)
server.send_message(msg)
print(f"[{timestamp}] Email sent: {status}")
except Exception as e: print(f"[ERROR] Email failed: {str(e)}")
# Main monitoring loop try:
print("Starting integrated soil monitoring...") last_check = time.time()
while True:
current_state = GPIO.input(SENSOR_CHANNEL) status = "DRY" if current_state else "WET"
# Send email at scheduled intervals if (time.time() - last_check) >= CHECK_INTERVAL: send_alert(status) last_check = time.time()
	time.sleep(60)	# Check every minute
except KeyboardInterrupt: GPIO.cleanup()
print("\nMonitoring stopped. GPIO cleanup complete.")
import RPi.GPIO as GPIO import time
# GPIO Configuration
SENSOR_CHANNEL = 4	# GPIO4 (Physical Pin 7)
GPIO.setmode(GPIO.BCM)
GPIO.setup(SENSOR_CHANNEL, GPIO.IN)
def sensor_callback(channel):
"""
Callback function triggered when moisture state changes.
Prints status to console.
""" if GPIO.input(channel): print("[STATUS] Soil is DRY. Water needed!") else:
print("[STATUS] Soil is WET. No action required.")
# Event detection setup
GPIO.add_event_detect(SENSOR_CHANNEL,	GPIO.BOTH, bouncetime=300)
GPIO.add_event_callback(SENSOR_CHANNEL, sensor_callback)
try:
	print("Soil	Moisture	Monitoring	Started	(Ctrl+C	to
exit)...") while True:
	time.sleep(1)	# Reduce CPU usage
except KeyboardInterrupt: GPIO.cleanup()
print("\nMonitoring stopped. GPIO cleanup complete.")
import smtplib
from email.message import EmailMessage
# Email Configuration (REPLACE THESE VALUES)
SENDER_EMAIL = "3022958276@qq.com"
SENDER_PASSWORD = "123"	# App-specific password
RECIPIENT_EMAIL = "rslinnn@gmail.com"
SMTP_SERVER = "smtp.office365.com"	# Outlook SMTP SMTP_PORT = 587
def send_email(subject, body): """
Sends an email notification using SMTP.
"""
# Create email message msg = EmailMessage() msg.set_content(body) msg['Subject'] = subject
msg['From'] = SENDER_EMAIL msg['To'] = RECIPIENT_EMAIL # Connect to SMTP server
try:
with smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
as server:
server.starttls()
server.login(SENDER_EMAIL,
SENDER_PASSWORD)
server.send_message(msg)
print("[EMAIL] Notification sent successfully.")
except Exception as e: print(f"[ERROR] Email failed: {str(e)}")
# Test email (uncomment to debug)
# send_email("Test Subject", "Hello from Raspberry Pi!")
