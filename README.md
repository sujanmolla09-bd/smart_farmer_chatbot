#!/usr/bin/env python3
import os
import time
import google.generativeai as gemini

class SmartFarmerSystem:
    def __init__(self):
        self.project_id = "ICT-STUP-2025-332630" # আপনার iDEA প্রজেক্ট আইডি
        self.location = "BSCIC Industrial Area, Thakurgaon"
        
        # জেমিনি এআই কনফিগারেশন
        # নোটপ্যাড থেকে নিয়ে আপনার আসল API Key এখানে বসাবেন
        self.api_key = os.getenv("GEMINI_API_KEY", "YOUR_COPIED_GEMINI_API_KEY")
        
        if self.api_key != "YOUR_COPIED_GEMINI_API_KEY":
            gemini.configure(api_key=self.api_key)
            self.ai_model = gemini.GenerativeModel('gemini-pro')
        else:
            self.ai_model = None

        # এআই-এর জন্য কাস্টম সিস্টেম গাইডলাইন (কৃষকদের সঠিক পরামর্শ দিতে)
        self.ai_context = """
        Role: Expert Agricultural AI Assistant (স্মার্ট কৃষক সহকারী)
        Project: Smart Agro Hub, Thakurgaon, Bangladesh.
        Language: Always respond in clear, empathetic, and professional Bengali (বাংলা).
        
        Instructions:
        1. Provide precise solutions for local crops (Rice, Wheat, Potato, Mustard).
        2. Give advice on fertilizer dosage (Urea, TSP, MoP), pest control, and soil health.
        3. Keep the language humble and highly supportive for rural farmers.
        """

    def ask_ai_assistant(self, farmer_query):
        """কৃষকের প্রশ্নের উত্তর জেমিনি এআই এর মাধ্যমে জেনারেট করা"""
        if not self.ai_model:
            return "সিস্টেম ত্রুটি: দয়া করে কোডের ভেতর আপনার সঠিক Gemini API Key-টি বসান।"
            
        full_prompt = f"{self.ai_context}\nFarmer Question: {farmer_query}\nAssistant Response:"
        try:
            response = self.ai_model.generate_content(full_prompt)
            return response.text
        except Exception as e:
            return f"দুঃখিত, এআই ইঞ্জিনে কানেক্ট করা যাচ্ছে না। এরর: {str(e)}"

    def run_farmer_interface(self):
        """স্মার্ট কৃষক ইন্টারেক্টিভ চ্যাটবট লুপ"""
        print(f"\n==================================================================")
        print(f"🌾 SMART FARMER CHATBOT ENGINE: [iDEA Project: {self.project_id}]")
        print(f"📍 Region: {self.location}")
        print(f"==================================================================")
        print("সিস্টেম স্ট্যাটাস: অনলাইন। (চ্যাট বন্ধ করতে 'exit' লিখুন)\n")
        
        if not self.ai_model or self.api_key == "YOUR_COPIED_GEMINI_API_KEY":
            print("⚠️ [সতর্কবার্তা]: জেমিনি API Key সেট করা নেই।")
            print("কোডের 'YOUR_COPIED_GEMINI_API_KEY' লেখাটি পরিবর্তন করে আপনার চাবিটি বসান।\n")
            return

        print("🤖 কৃষক সহকারী: আসসালামু আলাইকুম সুজন ভাই! আমাদের ঠাকুরগাঁও জোনের কৃষকদের আজ কীভাবে সাহায্য করতে পারি?")
        print("-" * 66)

        while True:
            farmer_input = input("\n👨‍🌾 কৃষক (প্রশ্ন লিখুন): ")
            if farmer_input.lower() == 'exit':
                print("\n==================================================================")
                print("Status: SAFE_LOGOUT - স্মার্ট কৃষক সহকারী সিস্টেম বন্ধ করা হয়েছে।")
                print("==================================================================")
                break
                
            if not farmer_input.strip():
                continue
                
            print("⏳ এআই প্রসেস করছে, অনুগ্রহ করে অপেক্ষা করুন...")
            time.sleep(0.5) # রিয়েল-টাইম ফিল দেওয়ার জন্য সামান্য বিরতি
            
            ai_reply = self.ask_ai_assistant(farmer_input)
            print(f"\n🤖 এআই সহকারী:\n{ai_reply}")
            print("-" * 50)

if __name__ == "__main__":
    # ইঞ্জিন চালু করা
    farmer_engine = SmartFarmerSystem()
    farmer_engine.run_farmer_interface()
