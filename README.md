import cv2
import os
import string

# Correct way to define the image path
image_path = r"C:\Users\Y.PAPA\Downloads\Stenography-main\Stenography-main\pic.jpg"

# Check if the image file exists
if not os.path.exists(image_path):
    print("Error: Image file does not exist!")
    exit()

# Read the image
img = cv2.imread(image_path)
if img is None:
    print("Error: Failed to load the image. Check the file format and path.")
    exit()

msg = input("Enter secret message: ")
password = input("Enter a passcode: ")

d = {}
c = {}

for i in range(255):
    d[chr(i)] = i
    c[i] = chr(i)

m = 0
n = 0
z = 0

for i in range(len(msg)):
    if n >= img.shape[0] or m >= img.shape[1]:  # Prevent going out of bounds
        print("Error: Message too long for the image!")
        exit()
    img[n, m, z] = d[msg[i]]
    n = n + 1
    m = m + 1
    z = (z + 1) % 3

cv2.imwrite("encryptedImage.jpg", img)
os.system("start encryptedImage.jpg")  # Windows command to open the image

message = ""
n = 0
m = 0
z = 0

pas = input("Enter passcode for Decryption: ")
if password == pas:
    for i in range(len(msg)):
        if n >= img.shape[0] or m >= img.shape[1]:
            break
        message = message + c[img[n, m, z]]
        n = n + 1
        m = m + 1
        z = (z + 1) % 3
    print("Decryption message:", message)
else:
    print("YOU ARE NOT AUTHORIZED!")
