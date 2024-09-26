import random
import random
import nltk
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
import tkinter as tk
from tkinter import ttk, scrolledtext
from gtts import gTTS
import os
import speech_recognition as sr
from selenium import webdriver
from selenium.common import TimeoutException
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import datetime as dt
import time
import pywhatkit as pwk  # For WhatsApp functionality
from tkinter import messagebox





from PIL import Image, ImageTk



greetings = {"hello": "KIKI welcomes you", "hi": "KIKI welcomes you", "hey": "KIKI welcomes you", "greetings": "KIKI welcomes you"}
farewells = {"bye": "THANK YOU", "goodbye": "THANK YOU", "see you later": "THANK YOU", "take care": "THANK YOU"}

# Simple conversation responses
responses = {
    "how are you": "I'm doing well, thank you!",
    "what's your name": "I'm a chatbot created by KIKI.",
    "what are you doing": "I am assisting you.",
    "what can you do": "I can help you with basic information and have simple conversations.",
    "who created you": "I was created by OpenAI, a leading AI research organization.",
    "tell me a joke": "Why don't scientists trust atoms? Because they make up everything!",
    "what is the meaning of life": "The meaning of life is a philosophical question. Many people have different beliefs about it.",
    "how old are you": "As an AI, I don't have an age. I exist purely in the digital realm!",
    "where do you live": "I'm an AI, so I don't have a physical location. But you can find me on various platforms!",
    "what's the weather like today": "I'm sorry, but I don't have access to real-time data. You can check the weather on a weather website or app.",
    "do you have any siblings": "As an AI chatbot, I don't have siblings, but I have many other AI friends!",
    "what is the capital of India": "The capital of India is Delhi.",
    "default": "Sorry, I don't understand. Can you please rephrase or ask something else?",
}

# Function to tokenize and clean user input
def preprocess_input(user_input):
    # Tokenize the user input
    tokens = word_tokenize(user_input.lower())
    # Remove stopwords
    stop_words = set(stopwords.words("english"))
    cleaned_tokens = [token for token in tokens if token not in stop_words]
    return cleaned_tokens

# Function to get the chatbot's response
def get_response(user_input):
    cleaned_input = preprocess_input(user_input)
    for word in cleaned_input:
        if word in greetings:
            return greetings[word]
        elif word in farewells:
            return farewells[word]
    return responses.get(user_input, responses["default"])

def on_audio_input_button():
    recognizer = sr.Recognizer()

    # Set a lower energy threshold to make the recognizer more sensitive to audio input
    recognizer.energy_threshold = 300

    with sr.Microphone() as source:
        print("Listening...")
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)

    try:
        user_input = recognizer.recognize_google(audio).lower()
        print("You: " + user_input)
        process_user_input(user_input)
    except sr.UnknownValueError:
        print("Sorry, I couldn't understand your speech.")
    except sr.RequestError:
        print("Sorry, there was an issue connecting to the speech recognition service.")

def process_user_input(user_input):
    global chat_history  # Declare chat_history as a global variable
    chat_history.insert(tk.END, "You: " + user_input + "\n")

    if user_input.strip() == "speak":
        chat_history.insert(tk.END, "Chatbot: To use voice input, click the 'Speak' button.\n\n")
    else:
        response = get_response(user_input).lower()
        chat_history.insert(tk.END, "Chatbot: " + response + "\n\n")

        # Convert the chatbot's response to speech using gTTS
        chatbot_response_tts = gTTS(text=response, lang="en")
        chatbot_response_tts.save("response.mp3")
        os.system("start response.mp3")

def chat():
    global user_input_entry  # Declare user_input_entry as a global variable
    user_input = user_input_entry.get().strip()
    process_user_input(user_input)
    user_input_entry.delete(0, tk.END)

def initialize_driver():
    global driver
    driver = webdriver.Chrome()
    driver.get("https://www.instagram.com/accounts/login/")
    time.sleep(10)

def log_in(username_entry=None,password_entry=None):
    USERNAME = username_entry.get()
    PASSWORD = password_entry.get()

    username_element = driver.find_element(By.XPATH, "//input")
    username_element.send_keys(USERNAME)

    password_element = driver.find_element(By.XPATH, "//input[@type='password']")
    driver.find_elements(By.TAG_NAME, "button")
    password_element.send_keys(PASSWORD)
    time.sleep(2)

    login_button = driver.find_element(By.XPATH, "/html/body/div[2]/div/div/div[2]/div/div/div/div[1]/section/main/div/div/div[1]/div[2]/form/div/div[3]")
    login_button.click()
    time.sleep(2)

def logout():
    driver.quit()
    root.destroy()

# ... (Previous code remains the same) ...
def follow_user(username):
    search_url = "https://www.instagram.com/" + username + "/"
    driver.get(search_url)

    # Wait for the Follow button to be present on the page
    wait = WebDriverWait(driver, 30)
    try:
        follow_button = wait.until(EC.presence_of_element_located((By.XPATH, "//button[@type='Follow']")))
    except TimeoutException as ex:
        print("TimeoutException: The Follow button was not found within the specified time.")
        return

    print("Follow Button Found:", follow_button)

    follow_button.click()
    time.sleep(10)

    # Click the Follow button
    follow_button = driver.find_element(By.XPATH, "//button[contains(text(), 'Follow')]")
    follow_button.click()


def create_instagram_tab(tab):
    # Create Instagram related elements
    instagram_frame = ttk.Frame(tab)
    instagram_frame.pack()

    # Load the Instagram logo image
    img = Image.open(r"C:\Users\hp\Downloads\Telegram Desktop\instagram_PNG10.png")
    img = img.resize((500, 300), Image.LANCZOS)
    instagram_logo = ImageTk.PhotoImage(img)

    # Add an image label for Instagram
    instagram_label = tk.Label(instagram_frame, image=instagram_logo)
    instagram_label.image = instagram_logo  # Keep a reference to the image to prevent it from being garbage collected
    instagram_label.pack()

    username_label = tk.Label(instagram_frame, text="Username:")
    username_label.pack()
    username_entry = tk.Entry(instagram_frame)
    username_entry.pack()

    password_label = tk.Label(instagram_frame, text="Password:")
    password_label.pack()
    password_entry = tk.Entry(instagram_frame, show="*")
    password_entry.pack()

    login_button = tk.Button(instagram_frame, text="Login to Instagram", command=lambda: log_in_instagram(username_entry.get(), password_entry.get(), instagram_frame))
    login_button.pack()

    follow_label = tk.Label(instagram_frame, text="Enter Username to view user profile:")
    follow_label.pack()
    follow_entry = tk.Entry(instagram_frame)
    follow_entry.pack()

    follow_button = tk.Button(instagram_frame, text="View User", command=lambda: follow_user(follow_entry.get()))
    follow_button.pack()


def log_in_instagram(username, password, instagram_frame):
    # Initialize the driver and login to Instagram
    initialize_driver()
    log_in(username, password, instagram_frame)

def log_in(username, password, instagram_frame=None):
    username_element = driver.find_element(By.XPATH, "//input[@name='username']")
    username_element.send_keys(username)

    password_element = driver.find_element(By.XPATH, "//input[@name='password']")
    password_element.send_keys(password)

    login_button = driver.find_element(By.XPATH, "//button[@type='submit']")
    login_button.click()
    time.sleep(2)

    # Pack the Instagram frame after logging in
    if instagram_frame:
        instagram_frame.pack()

def create_chatbot_tab(tab):
    # Create Chatbot related elements
    chatbot_frame = ttk.Frame(tab)
    chatbot_frame.pack()

    img = Image.open(r"C:\Users\hp\Downloads\Telegram Desktop\chat-bot-logo-bubble-talk-messenger-ai-robot-mascot-vector.jpg")
    img = img.resize((500, 300), Image.LANCZOS)
    chatbot_logo = ImageTk.PhotoImage(img)

    # Add an image label for Chatbot
    chatbot_label = tk.Label(chatbot_frame, image=chatbot_logo)
    chatbot_label.image = chatbot_logo  # Keep a reference to the image to prevent it from being garbage collected
    chatbot_label.pack()

    chat_history_label = tk.Label(chatbot_frame, text="Chat History:")
    chat_history_label.pack()

    global chat_history
    chat_history = scrolledtext.ScrolledText(chatbot_frame, width=60, height=15)
    chat_history.pack()

    user_input_label = tk.Label(chatbot_frame, text="Your Message:")
    user_input_label.pack()

    global user_input_entry
    user_input_entry = tk.Entry(chatbot_frame, width=50)
    user_input_entry.pack()

    send_button = tk.Button(chatbot_frame, text="Send", command=chat)
    send_button.pack()

    audio_input_button = tk.Button(chatbot_frame, text="Speak", command=on_audio_input_button)
    audio_input_button.pack()

def switch_tab(tab_index):
    notebook.select(tab_index)

def send_message():
    phone_number = phone_entry.get()
    text = message_entry.get()
    hour = hour_entry.get()
    minute = minute_entry.get()

    try:
        # Convert hour and minute to integers
        hour = int(hour)
        minute = int(minute)

        # Sending the message through WhatsApp Web
        pwk.sendwhatmsg(f"+91{phone_number}", text, hour, minute)
        print("Message scheduled successfully!")
    except ValueError:
        messagebox.showerror("Error", "Invalid time format. Please enter valid numbers for hour and minute.")
    except Exception as e:
        messagebox.showerror("Error", f"An error occurred: {e}")


global whatsapp_frame


def create_whatsapp_tab(tab):
    whatsapp_frame = ttk.Frame(tab)
    whatsapp_frame.pack()

    img = Image.open(r"C:\Users\hp\Downloads\whatsapp logo.png")
    img = img.resize((500, 300), Image.LANCZOS)
    whatsapp_logo = ImageTk.PhotoImage(img)

    # Add an image label for WhatsApp
    whatsapp_label = tk.Label(whatsapp_frame, image=whatsapp_logo)
    whatsapp_label.image = whatsapp_logo  # Keep a reference to the image to prevent it from being garbage collected
    whatsapp_label.pack()

    # Create WhatsApp related elements
    phone_label = tk.Label(whatsapp_frame, text="Phone Number (with country code):")
    phone_label.pack()

    global phone_entry
    phone_entry = tk.Entry(whatsapp_frame, width=20)
    phone_entry.pack()

    message_label = tk.Label(whatsapp_frame, text="Message:")
    message_label.pack()

    global message_entry
    message_entry = tk.Entry(whatsapp_frame, width=50)
    message_entry.pack()

    hour_label = tk.Label(whatsapp_frame, text="Hour (0-23):")
    hour_label.pack()

    global hour_entry
    hour_entry = tk.Entry(whatsapp_frame, width=5)
    hour_entry.pack()

    minute_label = tk.Label(whatsapp_frame, text="Minute (0-59):")
    minute_label.pack()

    global minute_entry
    minute_entry = tk.Entry(whatsapp_frame, width=5)
    minute_entry.pack()

    send_whatsapp_button = tk.Button(whatsapp_frame, text="Send WhatsApp Message", command=send_message)
    send_whatsapp_button.pack()


# Declare whatsapp_frame as a global variable

def send_whatsapp_after_login():
    phone_number = phone_entry.get()
    message = message_entry.get()
    hour = int(hour_entry.get())
    minute = int(minute_entry.get())

    # Calculate the total seconds to wait before sending the message
    total_seconds = (hour * 3600) + (minute * 60) + 20

    # Use the after() method to schedule the message sending function
    root.after(total_seconds * 1000, lambda: send_whatsapp(phone_number, message))

def send_message():
    phone_number = phone_entry.get()
    text = message_entry.get()
    hour = hour_entry.get()
    minute = minute_entry.get()

    try:
        # Convert hour and minute to integers
        hour = int(hour)
        minute = int(minute)

        # Sending the message through WhatsApp Web
        pwk.sendwhatmsg(f"{phone_number}", text, hour, minute)
        print("Message scheduled successfully!")
    except ValueError:
        messagebox.showerror("Error", "Invalid time format. Please enter valid numbers for hour and minute.")
    except Exception as e:
        messagebox.showerror("Error", f"An error occurred: {e}")


instagram_frame=None
chatbot_frame=None



def show_first_tab():
    # Hide other frames and show the first tab with the image
    instagram_frame.pack_forget()
    chatbot_frame.pack_forget()
    whatsapp_frame.pack_forget()
    first_tab_frame.pack()

def show_instagram_frame():
    # Hide other frames and show the Instagram frame
    first_tab_frame.pack_forget()
    chatbot_frame.pack_forget()
    whatsapp_frame.pack_forget()
    instagram_frame.pack()

def show_chatbot_frame():
    # Hide other frames and show the Chatbot frame
    first_tab_frame.pack_forget()
    instagram_frame.pack_forget()
    whatsapp_frame.pack_forget()
    chatbot_frame.pack()

def show_whatsapp_frame():
    # Hide other frames and show the WhatsApp frame
    first_tab_frame.pack_forget()
    instagram_frame.pack_forget()
    chatbot_frame.pack_forget()
    whatsapp_frame.pack()

# Create the GUI
root = tk.Tk()
root.title("🤖_🤖MAC - THE HELPER🤖_🤖")
icon_img = Image.open(r"C:\Users\hp\Downloads\pexels-pixabay-60597.jpg")
icon_img = icon_img.resize((50, 50), Image.LANCZOS)
icon = ImageTk.PhotoImage(icon_img)

root.iconphoto(True, icon)
root.geometry("1000x800")

title_label = tk.Label(root, text="MAC THE HELPER", font=("ALGERIAN", 15, "italic"))
title_label.pack(fill=tk.BOTH, expand=True)

# Create a Notebook widget for the tabs
notebook = ttk.Notebook(root)
notebook.pack(fill=tk.BOTH, expand=True)

# Create tabs for Instagram, Chatbot, and WhatsApp
instagram_tab = ttk.Frame(notebook)
chatbot_tab = ttk.Frame(notebook)
whatsapp_tab = ttk.Frame(notebook)

notebook.add(instagram_tab, text="Instagram")
notebook.add(chatbot_tab, text="Chatbot")
notebook.add(whatsapp_tab, text="WhatsApp")

# Create elements for Instagram, Chatbot, and WhatsApp
create_instagram_tab(instagram_tab)
create_chatbot_tab(chatbot_tab)
create_whatsapp_tab(whatsapp_tab)

# Start the GUI event loop
root.mainloop()
