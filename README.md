import os
import google.generativeai as gemini

class SmartFarmerChatbot:
    def __init__(self, api_key):
        """জেমিনি এআই এবং কাস্টম প্রম্পট সেটআপ"""
        gemini.configure(api_key=api_key)
        # টেক্সট চ্যাটের জন্য gemini-pro মডেল ব্যবহার করা হয়েছে
        self.model = gemini.GenerativeModel('gemini-pro')
        
        # চ্যাটবটকে একটি নির্দিষ্ট রোল বা ব্যক্তিত্ব দেওয়া (System Instruction)
        self.system_context = """
        Role: Expert Agricultural AI Consultant & Smart Farmer Assistant.
        Project: Smart Agro Hub (BSCIC, Thakurgaon, Bangladesh).
        Language: Always respond in clear, helpful, and friendly Bengali (বাংলা).
        
        Instructions:
        1. Give practical farming advice suitable for northern Bangladesh (Thakurgaon/Dinajpur region).
        2. Provide solutions regarding crop diseases (rice, wheat, potato, mustard), fertilizer dosage, soil moisture, and modern IoT/Drone tech.
        3. Keep the tone encouraging, respectful, and easily understandable for local farmers.
        """

    def ask_bot(self, farmer_question):
        """কৃষকের প্রশ্নের উত্তর তৈরি করা"""
        # সিস্টেম কনটেক্সটের সাথে কৃষকের প্রশ্নটি যুক্ত করা
        full_prompt = f"{self.system_context}\nFarmer's Question: {farmer_question}\nAssistant:"
        
        try:
            response = self.model.generate_content(full_prompt)
            return response.text
        except Exception as e:
            return f"দুঃখিত সুজন ভাই, জেমিনি এআই কানেকশনে সমস্যা হয়েছে। এরর: {str(e)}"

# ==========================================
#  চ্যাটবট সিমুলেশন (লোকাল টার্মিনালে টেস্ট করার জন্য)
# ==========================================
if __name__ == "__main__":
    print("--- Smart Farmer Chatbot AI Engine ---")
    print("সিস্টেম চালু হচ্ছে... (কুইট করতে 'exit' লিখুন)\n")
    
    # আপনার নোটপ্যাডে সেভ করা জেমিনি এপিআই কী এখানে বসাবেন
    # অথবা এনভায়রনমেন্ট ভ্যারিয়েবল হিসেবে রাখতে পারেন
    GEMINI_API_KEY = os.getenv("GEMINI_API_KEY", "YOUR_COPIED_GEMINI_API_KEY")
    
    if GEMINI_API_KEY == "YOUR_COPIED_GEMINI_API_KEY":
        print("[Error] দয়া করে কোডের ভেতর 'YOUR_COPIED_GEMINI_API_KEY' পরিবর্তন করে আপনার আসল API Key বসান।")
    else:
        # চ্যাটবট অবজেক্ট তৈরি
        bot = SmartFarmerChatbot(api_key=GEMINI_API_KEY)
        print("কৃষক সহকারী বট প্রস্তুত! আপনার প্রশ্নটি করুন।")
        print("="*40)
        
        # লাইভ চ্যাট লুপ
        while True:
            user_input = input("\nকৃষক/ইউজার: ")
            if user_input.lower() == 'exit':
                print("চ্যাটবট বন্ধ করা হচ্ছে। ধন্যবাদ!")
                break
                
            if user_input.strip() == "":
                continue
                
            print("বট প্রসেস করছে...")
            reply = bot.ask_bot(user_input)
            print(f"\nস্মার্ট অ্যাসিস্ট্যান্ট:\n{reply}")
            print("-" * 30)
