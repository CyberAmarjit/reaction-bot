import os
import time
import requests
import telebot

# ========== COLORS ==========
GREEN = "\033[92m"
RESET = "\033[0m"

# ========== FUNCTION: Read Data From File ==========
def read_from_file(file_path):
    """Read and return the first line from a text file."""
    with open(file_path, 'r') as file:
        return file.read().strip()

# ========== READ TOKEN AND CHAT ID ==========
BOT_TOKEN = read_from_file('token.txt')
CHAT_ID = read_from_file('chat_id.txt')

# Initialize TeleBot
bot = telebot.TeleBot(BOT_TOKEN)

# Base URL for Telegram API
BASE_URL = f"https://api.telegram.org/bot{BOT_TOKEN}"

# Reaction emoji (you can change it)
REACTION = "🔥"

# ========== BANNER ==========
def show_banner():
    """Display a green color banner with Amarjit name."""
    os.system("cls" if os.name == "nt" else "clear")  # Clear terminal screen

    banner = f"""
{GREEN}
     
  █████╗ ███╗   ███╗ █████╗ ██████╗      ██╗██╗████████╗
██╔══██╗████╗ ████║██╔══██╗██╔══██╗     ██║██║╚══██╔══╝
███████║██╔████╔██║███████║██████╔╝     ██║██║   ██║   
██╔══██║██║╚██╔╝██║██╔══██║██╔══██╗██   ██║██║   ██║   
██║  ██║██║ ╚═╝ ██║██║  ██║██║  ██║╚█████╔╝██║   ██║   
╚═╝  ╚═╝╚═╝     ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚════╝ ╚═╝   ╚═╝   
             AMARJIT - TELEGRAM BOT
{RESET}
    """
    print(banner)
    print("🚀 Telegram Reaction Bot Started...\n")

# ========== GET LATEST CHANNEL POST ==========
def get_latest_message_id():
    """
    Fetch the latest message from the channel using Telegram API.
    """
    url = f"{BASE_URL}/getUpdates"
    try:
        response = requests.get(url).json()
        if "result" in response and len(response["result"]) > 0:
            for update in reversed(response["result"]):
                try:
                    if "channel_post" in update and update["channel_post"]["chat"]["id"]:
                        return update["channel_post"]["message_id"]
                except KeyError:
                    continue
    except Exception as e:
        print("❌ Error fetching updates:", e)
    return None

# ========== ADD REACTION TO POST ==========
def add_reaction(message_id):
    """
    Add a reaction emoji to the detected channel post.
    """
    url = f"{BASE_URL}/setMessageReaction"
    data = {
        "chat_id": CHAT_ID,
        "message_id": message_id,
        "reaction": [{"type": "emoji", "emoji": REACTION}]
    }
    try:
        response = requests.post(url, json=data).json()
        return response
    except Exception as e:
        return {"ok": False, "error": str(e)}

# ========== MAIN FUNCTION ==========
def main():
    show_banner()
    last_message_id = None

    while True:
        message_id = get_latest_message_id()

        if message_id and message_id != last_message_id:
            print(f"📌 New Post Detected! Message ID: {message_id}")
            result = add_reaction(message_id)
            if result.get("ok"):
                print("✅ Reaction Added Successfully!")
            else:
                print("❌ Error Adding Reaction:", result)
            last_message_id = message_id

        time.sleep(3)  # Check every 3 seconds for new posts

# ========== START BOT ==========
if __name__ == "__main__":
    main()
