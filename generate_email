import pandas as pd
from openai import OpenAI

client = OpenAI()  # Ensure your environment has the right API key configured

# Load your CSV file
df = pd.read_csv("~/Documents/hackathon_1576_projects_with_assets_06_17_2025_16_58_11.csv")

# Function to generate personalized email using OpenAI API
def generate_email(name, title, description):
    prompt = f"""
        You are a helpful assistant writing personalized follow-up emails after a hackathon.

        Using the following information:
        - Team Member Name: {name}
        - Project Title: {title}
        - Project Description: {description}

        1. Summarize the project in 1 sentence (do NOT include the full description).
        2. Write a professional, friendly email inviting the team member to a short feedback session with the Product Team.
        3. Mention that the session is a follow-up after the hackathon and that they’ll receive a $50 gift card for participating.
        4. The email should include the project title, reference the summary of the idea, and be addressed to the team member by name.
        5. Do NOT include the full original project description in the email.

        Output only the full email text.

        Example style:
        - Warm, professional tone
        - Short paragraphs
        - Clear call to action

        Now write the email.
    """

    try:
        response = client.chat.completions.create(
            model="gpt-4",
            messages=[
                {"role": "system", "content": "You are a helpful assistant writing personalized emails."},
                {"role": "user", "content": prompt}
            ],
            temperature=0.7
        )

        return response.choices[0].message.content.strip()

    except Exception as e:
        return f"Error generating email: {e}"

# Generate emails and store in a list
emails = []
for i, row in df.iterrows():
    name = row.get("Team member names", "there")
    title = row.get("Project Title", "your project")
    desc = row.get("Description", "")
    
    print(f"Generating email for: {name} — {title}")
    email = generate_email(name, title, desc)
    emails.append(email)

# Add emails to the DataFrame
df['Personalized Email'] = emails

# Save the updated DataFrame to a CSV file
df.to_csv("output_with_emails.csv", index=False)
print("✅ Updated CSV saved as 'output_with_emails.csv'")
