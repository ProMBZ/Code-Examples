# Code-Examples
These are code examples of langgraph presentation

import userdata from google.colab
import requests

# ENV VARIABLES for security
SERPER_API_KEY = userdata.get("SERPER_API_KEY")
GEMINI_API_KEY = userdata.get("GEMINI_API_KEY")   # Hypothetical for Gemini
TAVILY_API_KEY = userdata.get("TAVILY_API_KEY")     # Hypothetical Tavily

#####################
# Tavily Node
#####################
def tavily_transform(input_text):
    """
    Hypothetical function using Tavily's library
    to transform/clean/format text.
    """
    # ... implement your Tavily logic here ...
    return f"[Tavily Pre-Processed]\n{input_text}"

#####################
# Serper Search Node
#####################
def serper_search(query):
    url = "https://google.serper.dev/search"
    headers = {
       "X-API-KEY": SERPER_API_KEY,
       "Content-Type": "application/json"
    }
    payload = {"q": query}

    response = requests.post(url, headers=headers, json=payload)
    data = response.json()
    # Return top snippet or entire result
    if "organic" in data and len(data["organic"]) > 0:
        return data["organic"][0].get("snippet", "")
    return "No search results found."

#####################
# Gemini Node (LLM)
#####################
def gemini_generate(prompt):
    """
    Pseudocode for calling Gemini's API.
    Replace with the actual method once Gemini's public API is available.
    """
    # Typically a POST request with your GEMINI_API_KEY
    # For demonstration, returning a mock response:
    return f"[Gemini Output]: Response to your prompt -> {prompt}"

#####################
# Human in the Loop (HITL)
#####################
def human_review(ai_output):
    """
    In a real system, this might pause and allow human intervention.
    For now, we simulate auto-approval.
    """
    print("HITL: Checking AI output ...")
    # e.g., a user interface prompt or form
    approval = True  # or False if the human disapproves
    return ai_output if approval else "[Human Overrode Output]"

#####################
# Orchestrate the Graph
#####################
def language_graph_pipeline(user_input):
    # 1. Tavily Transform
    tavily_output = tavily_transform(user_input)

    # 2. Serper Search (Optional) - if you want external context
    search_output = serper_search(user_input)

    # 3. Combine everything into a final prompt
    final_prompt = f"{tavily_output}\nSearch Context: {search_output}"

    # 4. Gemini Generation
    gemini_response = gemini_generate(final_prompt)

    # 5. Human Review
    final_answer = human_review(gemini_response)

    return final_answer

# Example usage:
if __name__ == "__main__":
    user_query = "What are the best AI chatbot frameworks in 2025?"
    result = language_graph_pipeline(user_query)
    print("\n=== Final Result ===")
    print(result)
