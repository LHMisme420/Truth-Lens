# src/analyzer/ai_analyzer.py (New consolidated AI analysis module)

import os
from openai import OpenAI
from typing import Dict, Any

# --- 1. CORE AI LENSES (The Prompts) ---

# These prompts define the mathematical and strategic approach of the Truth Lens
POWER_ANALYSIS_PROMPT = """
Analyze the following text from a power perspective. Identify:
Who benefits from the framing of this text?
What assumptions about power are baked into the text?
How does the text reinforce or challenge existing power structures?
Text: {text}
"""

SILENCE_ANALYSIS_PROMPT = """
Analyze the following text for what is left out. Identify:
Who or what is not mentioned that might be relevant?
What perspectives are missing?
What questions are left unanswered?
Text: {text}
"""

CONTEXT_ANALYSIS_PROMPT = """
Analyze the following text in its broader context. Identify:
What historical conditions might have led to this text?
What economic conditions might have influenced it?
What social or cultural factors are reflected?
Text: {text}
"""

# --- 2. THE ANALYZER CLASS (The Core Protocol) ---

class AIAnalyzer:
    """
    Analyzes text using the three core AI-powered lenses: Power, Silence, and Context.
    This acts as the higher-level analysis protocol for Truth Lens.
    """
    def __init__(self, model: str = "gpt-3.5-turbo"):
        """Initializes the OpenAI client and model settings."""
        api_key = os.getenv('OPENAI_API_KEY')
        if not api_key:
            raise ValueError("OPENAI_API_KEY environment variable not set.")
        
        self.client = OpenAI(api_key=api_key)
        self.model = model
        self.prompts = {
            'power': POWER_ANALYSIS_PROMPT,
            'silence': SILENCE_ANALYSIS_PROMPT,
            'context': CONTEXT_ANALYSIS_PROMPT
        }

    def _get_analysis_response(self, prompt: str, text: str) -> str:
        """Helper to call the OpenAI API with a specific prompt."""
        response = self.client.chat.completions.create(
            model=self.model,
            messages=[
                {"role": "user", "content": prompt.format(text=text)}
            ]
        )
        return response.choices[0].message.content

    def analyze(self, text: str) -> Dict[str, Any]:
        """
        Runs the full analysis using all three lenses.
        """
        if not text or len(text) < 10:
            return {"error": "Text too short for meaningful AI analysis."}

        analyses = {}

        for lens, prompt in self.prompts.items():
            try:
                analyses[lens] = self._get_analysis_response(prompt, text)
            except Exception as e:
                analyses[lens] = f"AI Analysis Failed: {type(e).__name__}"
        
        return analyses

# --- 3. CONVENIENCE FUNCTION ---

def analyze_text_with_ai(text: str) -> Dict[str, Any]:
    """
    Standalone function to quickly run the AI analysis.
    This replaces the 'from truth_lens.analyzer import analyze' function setup.
    """
    analyzer = AIAnalyzer()
    return analyzer.analyze(text)


if __name__ == '__main__':
    # Example usage for testing integrity
    test_text = "The Board of Directors has unanimously approved the new policy to optimize operational efficiency and maximize shareholder return."
    print("Running AI Truth Lens Analysis on Test Text...")
    results = analyze_text_with_ai(test_text)
    
    print("\n--- POWER ANALYSIS ---")
    print(results.get('power'))
    
    print("\n--- SILENCE ANALYSIS ---")
    print(results.get('silence'))

# Final structural cleanups to reflect this change:
# - Update the __init__.py in src/analyzer to include AIAnalyzer.
# - The web_interface.py and cli.py files must now import analyze_text_with_ai or instantiate AIAnalyzer.
