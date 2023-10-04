# WebuiSD
this should be replaced inside it. improving the user input

import os
import openai
import csv
from datetime import datetime

# Configure OpenAI API key
openai.api_key = ""

# Function to write to CSV
def write_to_csv(serial, input_text, output_text, date):
    file_exists = os.path.isfile('/content/drive/MyDrive/translation_log.csv')
    with open('/content/drive/MyDrive/translation_log.csv', 'a', newline='') as csvfile:
        fieldnames = ['Serial Number', 'Input', 'Output', 'Date']
        writer = csv.DictWriter(csvfile, fieldnames=fieldnames)

        if not file_exists:
            writer.writeheader()

        writer.writerow({'Serial Number': serial, 'Input': input_text, 'Output': output_text, 'Date': date})

# Initialize serial number
serial_number = 1

# Your chat or translation loop here
while True:
    user_input = input("Enter text to translate from Korean to English (or 'quit' to exit): ")
    if user_input.lower() == 'quit':
        break
    
    # Assuming you get the translated text in the variable `translated_text`
    response = openai.Completion.create(
        model="gpt-3.5-turbo-instruct-0914",
        prompt=f"Translate the text input from Korean to English of the following:{user_input}. After the translation, organize the output to make it easily readable such it should be similar to the input.",
        temperature=1,
        max_tokens=2048,
        top_p=1,
        frequency_penalty=0,
        presence_penalty=0
    )
    translated_text = response['choices'][0]['text'].strip()
    
    print(f'Translated Text: {translated_text}')
    
    # Get current date
    current_date = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    
    # Write to CSV
    write_to_csv(serial_number, user_input, translated_text, current_date)
    
    # Increment serial number
    serial_number += 1
