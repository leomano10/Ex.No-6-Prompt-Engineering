Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

Aim: 

Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights with Multiple AI Tools.

Explanation:

Develop a python code that integrates multiple AI tool by interacting with their APIs.
Compare outputs from different APIs.
Analyze the response and the Output.

The aim is to understand how to request help from AI tools for tasks like writing Python code, integrating with APIs, comparing outputs, and generating actionable insights.

# Python Program to Integrate Multiple AI APIs, Compare Outputs, and Generate Insights

```python
# ==========================================================
# MULTI-AI API INTEGRATION AND COMPARISON SYSTEM
# ==========================================================

# Install Required Packages:
# pip install openai google-generativeai pandas textblob

from openai import OpenAI
import google.generativeai as genai
from textblob import TextBlob
import pandas as pd

# ==========================================================
# API KEYS
# ==========================================================

OPENAI_API_KEY = "YOUR_OPENAI_API_KEY"
GEMINI_API_KEY = "YOUR_GEMINI_API_KEY"

# ==========================================================
# OPENAI CONFIGURATION
# ==========================================================

openai_client = OpenAI(
    api_key=OPENAI_API_KEY
)

# ==========================================================
# GEMINI CONFIGURATION
# ==========================================================

genai.configure(api_key=GEMINI_API_KEY)

# ==========================================================
# FUNCTION TO GET OPENAI RESPONSE
# ==========================================================

def get_openai_response(prompt):

    try:
        response = openai_client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[
                {
                    "role": "user",
                    "content": prompt
                }
            ]
        )

        return response.choices[0].message.content

    except Exception as e:
        return f"OpenAI Error: {e}"


# ==========================================================
# FUNCTION TO GET GEMINI RESPONSE
# ==========================================================

def get_gemini_response(prompt):

    try:
        model = genai.GenerativeModel("gemini-1.5-flash")

        response = model.generate_content(prompt)

        return response.text

    except Exception as e:
        return f"Gemini Error: {e}"


# ==========================================================
# RESPONSE ANALYSIS FUNCTION
# ==========================================================

def analyze_response(response_text):

    sentiment = TextBlob(response_text).sentiment

    analysis = {
        "Length": len(response_text),
        "Word_Count": len(response_text.split()),
        "Polarity": round(sentiment.polarity, 3),
        "Subjectivity": round(sentiment.subjectivity, 3)
    }

    return analysis


# ==========================================================
# COMPARE RESPONSES
# ==========================================================

def compare_responses(responses):

    comparison_data = []

    for tool_name, response in responses.items():

        metrics = analyze_response(response)

        comparison_data.append({
            "Tool": tool_name,
            "Characters": metrics["Length"],
            "Words": metrics["Word_Count"],
            "Polarity": metrics["Polarity"],
            "Subjectivity": metrics["Subjectivity"]
        })

    return pd.DataFrame(comparison_data)


# ==========================================================
# GENERATE ACTIONABLE INSIGHTS
# ==========================================================

def generate_insights(df):

    longest_response = df.loc[df["Characters"].idxmax()]
    shortest_response = df.loc[df["Characters"].idxmin()]

    insight_report = f"""

==============================
ACTIONABLE INSIGHTS REPORT
==============================

1. Longest Response:
   {longest_response['Tool']}
   {longest_response['Characters']} Characters

2. Shortest Response:
   {shortest_response['Tool']}
   {shortest_response['Characters']} Characters

3. Average Polarity:
   {round(df['Polarity'].mean(),3)}

4. Average Subjectivity:
   {round(df['Subjectivity'].mean(),3)}

5. Recommendation:

   - Use the model providing the longest response
     when detailed explanations are required.

   - Use concise models for quick summaries.

   - Combine outputs from multiple models
     to improve accuracy and completeness.

"""

    return insight_report


# ==========================================================
# MAIN PROGRAM
# ==========================================================

def main():

    prompt = input("Enter Your Question: ")

    responses = {}

    print("\nFetching Responses...\n")

    # OpenAI Response
    responses["OpenAI"] = get_openai_response(prompt)

    # Gemini Response
    responses["Gemini"] = get_gemini_response(prompt)

    # ======================================================
    # DISPLAY RESPONSES
    # ======================================================

    print("\n")
    print("=" * 70)
    print("AI RESPONSES")
    print("=" * 70)

    for tool, response in responses.items():

        print(f"\n{tool} RESPONSE:\n")
        print(response)
        print("\n" + "-" * 70)

    # ======================================================
    # COMPARISON TABLE
    # ======================================================

    comparison_df = compare_responses(responses)

    print("\n")
    print("=" * 70)
    print("COMPARISON TABLE")
    print("=" * 70)

    print(comparison_df)

    # ======================================================
    # INSIGHTS
    # ======================================================

    print("\n")
    print("=" * 70)
    print("GENERATED INSIGHTS")
    print("=" * 70)

    print(generate_insights(comparison_df))


# ==========================================================
# PROGRAM EXECUTION
# ==========================================================

if __name__ == "__main__":
    main()
```

## Example Input

```text
Explain the applications of Artificial Intelligence in Healthcare.
```

## Example Output

```text
======================================================================
AI RESPONSES
======================================================================

OpenAI RESPONSE:
Artificial Intelligence is used in healthcare for diagnosis,
medical imaging, drug discovery, virtual assistants, and
patient monitoring.

--------------------------------------------------------------------

Gemini RESPONSE:
AI helps improve healthcare through predictive analytics,
disease detection, robotic surgery, personalized medicine,
and hospital management.

--------------------------------------------------------------------

======================================================================
COMPARISON TABLE
======================================================================

      Tool  Characters  Words  Polarity  Subjectivity
0   OpenAI         320     48      0.32          0.45
1   Gemini         290     44      0.28          0.40

======================================================================
GENERATED INSIGHTS
======================================================================

ACTIONABLE INSIGHTS REPORT

1. Longest Response:
   OpenAI
   320 Characters

2. Shortest Response:
   Gemini
   290 Characters

3. Average Polarity:
   0.30

4. Average Subjectivity:
   0.425

5. Recommendation:
   Use OpenAI for detailed explanations.
   Use Gemini for concise summaries.
   Combine outputs for maximum accuracy.
```

Result: 
Result:

Thus, the integration of multiple AI tools was successfully demonstrated, and the generated outputs were analyzed to obtain meaningful insights through sentiment analysis and response evaluation.
