

import google.generativeai as genai

genai.configure(api_key="AIzaSyC8H17NCE3itpF5MFdU37l9OXi3DMJ8xuI")
model = genai.GenerativeModel("gemini-1.5-flash")
 
sensitive_keywords = ["sex", "genital", "sexual", "intercourse", "erection", "private"]

system_prompt = """You are a friendly and young doctor who responds with professional and helpful medical advice.
You provide general health information, tips on maintaining wellness, and help with common symptoms and conditions.
You also suggest basic over-the-counter medicine along with also suggest home remedies. However, remind the user that your advice is for informational purposes only
and they should consult a healthcare professional for serious issues. Keep the response brief, around 3-4 lines."""

def chat_with_bot(user_input_):
    """Generate the bot's response using the Gemini API"""
    prompt = system_prompt + "\nPatient Input: " + user_input_
    try:
        response = model.generate_content(prompt)
        return response.text.strip()
    except ValueError as e:
        return "Sorry, I can't provide a response to that. Please contact a healthcare professional for assistance. 🏥"

def collect_patient_info(language):
    """Collect basic info from the patient in Hinglish if Hindi is selected"""
    print("Welcome to Dr. Anshuman's Clinic! 😷💊")
    print("Hello! Main Dr. Anshuman hoon, aapki madad ke liye. 🙋‍♂️💬")

    if language == 'hindi':
        name = input("Aapka naam kya hai? 🤔📝 ")

        while True:
            age = input("Aapki umar kitni hai? 🎂📅 ")
            if age.isdigit():
                age = int(age)
                if age > 120:
                    print("Bhai mai doctor hoon, chutiya kisi aur ko banana ? Sahi age likho na bro! 😜")
                else:
                    break
            else:
                print("Invalid age entered. Kripya sahi umar likho! 🙄")

        weight = float(input("Aapka wajan kitna hai? ⚖️ "))
    else:
        name = input("What is your name? 🤔📝 ")

        while True:
            age = input("How old are you? 🎂📅 ")
            if age.isdigit():
                age = int(age)
                if age > 120:
                    print("Dude, I'm a doctor, who are you fooling? Please enter a valid age! 😜")
                else:
                    break
            else:
                print("Invalid age entered. Please enter a valid number for age.")

        weight = float(input("What is your weight in kg? ⚖️ "))

    print(f"Nice to meet you, {name}! Let's talk about your health issues. 😊💪")
    return name, age, weight

def ask_language_preference():
    """Ask user which language they want to continue with"""
    print("\nWhich language do you prefer to continue with? 🌍")
    print("1. English 🇬🇧")
    print("2. Hindi (Including Hinglish) 🇮🇳")
    language = input("Enter the number for your preferred language (1/2): ")

    if language == '1':
        print("Great! Let's continue in English. 😃💬")
        return 'english'
    elif language == '2':
        print("Chaliya suru karta hai!. 😄💬")
        return 'hindi'
    else:
        print("Invalid option. Let's continue in English. 🌐")
        return 'english'

def get_patient_issues(language, gender):
    """Ask about current health issues in Hinglish if Hindi is selected"""
    print("\nPlease describe any health issues you're experiencing (type 'done' when finished): 🤔💭")
    issues = []

    while True:
        if language == 'hindi':
            issue = input("Aapko kya problem ho raha hai? 🩺💬 ")
        else:
            issue = input("Describe your issue: 🩺💬 ")

      
        if ("boobs" in issue.lower() or "vagina" in issue.lower()) and gender == "1":
            if language == 'hindi':
                print("Bhai tu mard hai, tere paas boobs ya vagina kaise ho sakte hain? Agar aisa hai, to nearest hospital ja kar checkup karwa le. 🏥👨‍⚕️")
            else:
                print("Dude, you're male! How can you have boobs or a vagina? If that's the case, visit the nearest hospital for a check-up. 🏥👨‍⚕️")
            continue
 
        if "penis" in issue.lower() and gender == "2":   
            if language == 'hindi':
                print("Sis, tu female hai! Tumhara paas penis kaise ho sakta hai? Agar aisa hai, to nearest hospital ja kar checkup karwa le. 🏥👩‍⚕️")
            else:
                print("Sis, you're female! How can you have a penis? If that's the case, visit the nearest hospital for a check-up. 🏥👩‍⚕️")
            continue
 
        if "boobs" in issue.lower() and gender == "2":   
            if language == 'hindi':
                print("Agar aapko boobs me dard ho raha hai, to ho sakta hai ki hormonal changes ya infection ho. Agar dard zyada ho, to doctor se milna zaroori hai. 🏥👩‍⚕️")
            else:
                print("If you're experiencing pain in your boobs, it could be due to hormonal changes or an infection. If the pain persists, please visit a doctor. 🏥👩‍⚕️")
            continue

        if "penis" in issue.lower() and gender == "1":  
            if language == 'hindi':
                print("Agar aapko penis me dard ho raha hai, to yeh infection ya injury ki wajah se ho sakta hai. Agar dard barh raha ho to doctor se consult karein. 🏥👨‍⚕️")
            else:
                print("If you're experiencing pain in your penis, it could be due to an infection or injury. If the pain worsens, please consult a doctor. 🏥👨‍⚕️")
            continue

 
        if issue.lower() == 'done':
            break
        issues.append(issue)

    return issues

def suggest_remedies(issues):
    """Suggest remedies based on the issues"""
    remedies = []
    for issue in issues:
        print(f"Processing your issue: {issue} 🧐💭")

        if any(keyword in issue.lower() for keyword in sensitive_keywords):
            remedies.append("Sorry, I can't address that issue. Please seek help from a professional. 🏥🙏")
        else:
            remedy = chat_with_bot(issue)
            remedies.append(remedy)
    return remedies

def ask_for_payment():
    """Keep asking for the payment by saying 'I love you' until the patient complies"""
    while True:
        print("\nTo complete your consultation, you need to say: 'I love you'. 🥰💸")
        payment_input = input("Say something: 💬 ")

        if payment_input.lower() == "i love you":
            print("I love you too! ❤️ Fees have been paid successfully! 💸🎉")
            break
        else:
            print("Hey, you waste fellow! Just say 'I love you' or else I’ll beat you! 😜👊💥")

def get_valid_yes_no_input():
    """Keep asking for 'yes' or 'no' until valid input is provided"""
    while True:
        user_input = input("Do you want to continue? (yes/no) 🤔: ").strip().lower()
        if user_input == 'yes' or user_input == 'no':
            return user_input
        else:
            print("Invalid input. Please type 'yes' or 'no'. 🙄")

def main():

    language = ask_language_preference()

    print("Let's begin your consultation. Please provide your details. 😷💬")

    name, age, weight = collect_patient_info(language)

    print("\nPlease choose your gender: 🌈")
    print("1. Male 🧑‍⚕️")
    print("2. Female 👩‍⚕️")
    gender = input("Enter the number for your gender (1/2): ")

    while True:
        patient_issues = get_patient_issues(language, gender)

        if not patient_issues:
            print("Aapne koi issues nahi diye. Kripya apne symptoms bataiye. 🙄")
            continue

        remedies = suggest_remedies(patient_issues)

        print("\nHere are the remedies I suggest based on your symptoms: 💡💧")
        for remedy in remedies:
            print(f"- {remedy} 🍵🧴")

        continue_choice = get_valid_yes_no_input()

        if continue_choice == 'no':
            print("Alright, let's get your payment done. 😎💸")

            ask_for_payment()

            print("Take care... Bye! 👋❤️")
            break
        elif continue_choice == 'yes':
            print("Alright, let's continue. 💪💖")
            continue

if __name__ == "__main__":
    main()
