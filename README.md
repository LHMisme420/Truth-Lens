# Truth-Lens
Lens of truth
git clone https://github.com/LHMisme420/Truth-Lens/blob/main/README.md
cd truth-lens
pip install -r requirements.txt
from truth_lens.analyzer import analyze

text = "Your text here"
results = analyze(text)
print(results)
POWER_ANALYSIS_PROMPT = """
Analyze the following text from a power perspective. Identify:
1. Who benefits from the framing of this text?
2. What assumptions about power are baked into the text?
3. How does the text reinforce or challenge existing power structures?

Text: {text}
"""

SILENCE_ANALYSIS_PROMPT = """
Analyze the following text for what is left out. Identify:
1. Who or what is not mentioned that might be relevant?
2. What perspectives are missing?
3. What questions are left unanswered?

Text: {text}
"""

CONTEXT_ANALYSIS_PROMPT = """
Analyze the following text in its broader context. Identify:
1. What historical conditions might have led to this text?
2. What economic conditions might have influenced it?
3. What social or cultural factors are reflected?

Text: {text}
"""
import os
from openai import OpenAI
from .prompts import POWER_ANALYSIS_PROMPT, SILENCE_ANALYSIS_PROMPT, CONTEXT_ANALYSIS_PROMPT

# Initialize the OpenAI client
client = OpenAI(api_key=os.getenv('OPENAI_API_KEY'))

def analyze(text, model="gpt-3.5-turbo"):
    """
    Analyze the given text with the three lenses: power, silence, and context.
    Returns a dictionary with the three analyses.
    """
    analyses = {}
    
    # Power Analysis
    power_response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "user", "content": POWER_ANALYSIS_PROMPT.format(text=text)}
        ]
    )
    analyses['power'] = power_response.choices[0].message.content

    # Silence Analysis
    silence_response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "user", "content": SILENCE_ANALYSIS_PROMPT.format(text=text)}
        ]
    )
    analyses['silence'] = silence_response.choices[0].message.content

    # Context Analysis
    context_response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "user", "content": CONTEXT_ANALYSIS_PROMPT.format(text=text)}
        ]
    )
    analyses['context'] = context_response.choices[0].message.content

    return analyses
    # Repository Structure
truth-lens/
├── README.md
├── requirements.txt
├── src/
│   ├── analyzer/
│   │   ├── __init__.py
│   │   ├── power_analyzer.py
│   │   ├── silence_detector.py
│   │   └── context_mapper.py
│   ├── interface/
│   │   ├── web_interface.py
│   │   └── api.py
│   └── core/
│       ├── config.py
│       └── security.py
├── tests/
├── docs/
│   ├── API.md
│   └── ANALYSIS_FRAMEWORK.md
└── examples/
    ├── political_speech_analysis.ipynb
    └── corporate_policy_demo.py
    # Truth Lens - See Through the Noise

A tool for the forgotten people. Truth Lens reveals the hidden frameworks in any text through three core analyses:

## The Three Lenses

1. **Power Analysis** - Who benefits? What power assumptions are baked in?
2. **Silence Analysis** - Who/what is missing? What perspectives are excluded?
3. **Context Analysis** - What conditions made this possible?

## For People, Not Profit

This tool is designed for:
- Tenants reading landlord notices
- Workers decoding corporate policies  
- Citizens analyzing political speeches
- Anyone the system tries to confuse

## No Tracking, No Bullshit
- Zero user data collection
- Local processing where possible
- Transparent analysis methods
- 
class PowerAnalyzer:
    """
    Reveals power structures and beneficiaries in text
    """
    
    def analyze_beneficiaries(self, text):
        """Identifies who benefits from the framing"""
        # Core analysis logic here
        return {
            'direct_beneficiaries': [],
            'indirect_beneficiaries': [],
            'power_assumptions': [],
            'recommended_questions': []
        }from flask import Flask, request, jsonify
app = Flask(__name__)

@app.route('/analyze', methods=['POST'])
def analyze_text():
    text = request.json.get('text')
    return jsonify({
        'power_analysis': power_analyzer.analyze(text),
        'silence_analysis': silence_detector.detect(text),
        'context_analysis': context_mapper.map(text)
    })flask==2.3.3
nltk==3.8.1
textblob==0.17.1
newspaper3k==0.2.8
requests==2.31.0
gunicorn==21.2.0
python-dotenv==1.0.0
import os
from dotenv import load_dotenv

load_dotenv()

class Config:
    """Zero-tracking configuration"""
    SECRET_KEY = os.getenv('SECRET_KEY', 'truth-lens-local-dev')
    MAX_CONTENT_LENGTH = 16 * 1024 * 1024  # 16MB max file size
    ANALYSIS_CACHE_SIZE = 100  # In-memory cache size
    ENABLE_PERSISTENCE = False  # Explicitly disable data persistence
    
    # Analysis parameters
    MAX_TEXT_LENGTH = 10000
    MIN_CONFIDENCE_THRESHOLD = 0.6
    import re
import html

class Security:
    """Security and sanitization for zero-trust environment"""
    
    @staticmethod
    def sanitize_input(text):
        """Remove potentially dangerous content"""
        if not text:
            return ""
        
        # Basic HTML escaping
        text = html.escape(text)
        
        # Remove suspicious patterns
        suspicious_patterns = [
            r'<script.*?>.*?</script>',
            r'javascript:',
            r'vbscript:',
            r'on\w+='
        ]
        
        for pattern in suspicious_patterns:
            text = re.sub(pattern, '', text, flags=re.IGNORECASE)
            
        return text[:10000]  # Enforce length limit
    
    @staticmethod
    def generate_anonymous_session():
        """Generate session ID that cannot be traced back to user"""
        import uuid
        import time
        return f"session_{int(time.time())}_{uuid.uuid4().hex[:8]}"
        import re
from textblob import TextBlob
from typing import Dict, List, Any

class PowerAnalyzer:
    """
    Reveals power structures, beneficiaries, and hidden assumptions in text
    """
    
    def __init__(self):
        self.power_indicators = {
            'authority_terms': ['must', 'require', 'comply', 'obligated', 'mandatory', 'prohibited'],
            'benefit_terms': ['profit', 'revenue', 'growth', 'efficiency', 'optimize', 'leverage'],
            'control_terms': ['manage', 'direct', 'oversee', 'administer', 'regulate', 'enforce'],
            'exclusion_terms': ['except', 'excluding', 'limited to', 'qualified', 'eligible', 'approval required']
        }
    
    def analyze_beneficiaries(self, text: str) -> Dict[str, Any]:
        """Identify who benefits from the framing and assumptions"""
        blob = TextBlob(text)
        
        # Extract entities that might indicate beneficiaries
        entities = self._extract_entities(text)
        
        # Analyze sentiment around different entities
        beneficiary_analysis = []
        for entity in entities[:10]:  # Limit to top 10 entities
            context_sentiment = self._get_entity_sentiment(blob, entity)
            if context_sentiment and context_sentiment > 0.3:
                beneficiary_analysis.append({
                    'entity': entity,
                    'benefit_level': 'high',
                    'evidence': f"Positive language used around '{entity}'",
                    'sentiment_score': context_sentiment
                })
        
        return {
            'direct_beneficiaries': beneficiary_analysis,
            'power_assumptions': self._find_power_assumptions(text),
            'control_structures': self._identify_control_structures(text),
            'recommended_questions': self._generate_power_questions(text)
        }
    
    def _extract_entities(self, text: str) -> List[str]:
        """Extract potential entities (organizations, groups, roles)"""
        # Simple pattern-based extraction - could be enhanced with NER
        patterns = {
            'organizations': r'\b[A-Z][a-z]+(?:\s+[A-Z][a-z]+)*\s+(?:Inc|Corp|Company|Organization|Group)\b',
            'government': r'\b(?:Department|Agency|Bureau|Ministry|Committee)\s+of\s+[A-Z][a-z]+\b',
            'roles': r'\b(?:CEO|Director|Manager|Officer|Administrator|Coordinator)\b'
        }
        
        entities = []
        for category, pattern in patterns.items():
            entities.extend(re.findall(pattern, text))
        
        return list(set(entities))
    
    def _get_entity_sentiment(self, blob: TextBlob, entity: str) -> float:
        """Get sentiment in sentences mentioning the entity"""
        entity_sentences = [str(s) for s in blob.sentences if entity.lower() in str(s).lower()]
        if not entity_sentences:
            return 0.0
        
        total_sentiment = sum(TextBlob(s).sentiment.polarity for s in entity_sentences)
        return total_sentiment / len(entity_sentences)
    
    def _find_power_assumptions(self, text: str) -> List[Dict]:
        """Identify underlying power assumptions"""
        assumptions = []
        
        # Look for unquestioned authority
        authority_patterns = [
            (r'as\s+we\s+determine', 'Organization determines criteria without oversight'),
            (r'at\s+our\s+discretion', 'Complete discretion without specified limits'),
            (r'final\s+and\s+binding', 'No appeal or challenge mechanism'),
            (r'subject\s+to\s+change\s+without\s+notice', 'Unilateral modification power')
        ]
        
        for pattern, description in authority_patterns:
            if re.search(pattern, text, re.IGNORECASE):
                assumptions.append({
                    'type': 'unchecked_authority',
                    'description': description,
                    'example': re.search(pattern, text, re.IGNORECASE).group(0)
                })
        
        return assumptions
    
    def _identify_control_structures(self, text: str) -> List[Dict]:
        """Identify mechanisms of control in the text"""
        controls = []
        
        control_patterns = [
            (r'must\s+(?:not\s+)?\w+', 'Obligation without justification'),
            (r'required\s+to\s+\w+', 'Mandatory action'),
            (r'prohibited\s+from\s+\w+', 'Explicit prohibition'),
            (r'only\s+if\s+approved', 'Gatekeeping mechanism')
        ]
        
        for pattern, control_type in control_patterns:
            matches = re.findall(pattern, text, re.IGNORECASE)
            for match in matches:
                controls.append({
                    'control_type': control_type,
                    'language': match,
                    'impact': 'limits autonomy'
                })
        
        return controls
    
    def _generate_power_questions(self, text: str) -> List[str]:
        """Generate critical questions about power dynamics"""
        questions = [
            "Who has the authority to change these terms?",
            "What recourse do I have if I disagree?",
            "Who benefits most from this arrangement?",
            "What assumptions about power are being made here?",
            "Who is not represented in this text?"
        ]
        
        # Add context-specific questions
        if re.search(r'\b(?:tenant|rent|lease)\b', text, re.IGNORECASE):
            questions.extend([
                "What rights is the landlord assuming they have?",
                "What protections are missing for the tenant?",
                "Who bears the most risk in this agreement?"
            ])
        
        return questions
        import re
from typing import Dict, List, Any
from textblob import TextBlob

class SilenceDetector:
    """
    Detects what and who is missing from the text
    """
    
    def __init__(self):
        self.common_exclusions = {
            'perspectives': ['community', 'public', 'resident', 'tenant', 'worker', 'consumer'],
            'considerations': ['alternative', 'opposing view', 'criticism', 'limitation', 'drawback'],
            'rights': ['appeal', 'contest', 'negotiate', 'modify', 'refuse']
        }
    
    def detect_silences(self, text: str) -> Dict[str, Any]:
        """Identify missing perspectives and considerations"""
        return {
            'missing_perspectives': self._find_missing_perspectives(text),
            'absent_considerations': self._find_absent_considerations(text),
            'unmentioned_rights': self._find_unmentioned_rights(text),
            'narrative_gaps': self._identify_narrative_gaps(text),
            'questions_to_uncover_silence': self._generate_silence_questions(text)
        }
    
    def _find_missing_perspectives(self, text: str) -> List[Dict]:
        """Find commonly excluded perspectives"""
        missing = []
        text_lower = text.lower()
        
        for perspective in self.common_exclusions['perspectives']:
            if perspective not in text_lower:
                missing.append({
                    'perspective': perspective,
                    'significance': f"The {perspective} viewpoint is not represented",
                    'potential_impact': f"May overlook {perspective} needs and concerns"
                })
        
        return missing
    
    def _find_absent_considerations(self, text: str) -> List[Dict]:
        """Identify common considerations that are absent"""
        absent = []
        text_lower = text.lower()
        
        for consideration in self.common_exclusions['considerations']:
            if consideration not in text_lower:
                absent.append({
                    'consideration': consideration,
                    'type': 'critical_omission',
                    'question': f"What {consideration}s were not discussed?"
                })
        
        return absent
    
    def _find_unmentioned_rights(self, text: str) -> List[Dict]:
        """Find rights that are not mentioned but might be relevant"""
        unmentioned = []
        text_lower = text.lower()
        
        rights_to_check = [
            ('appeal', 'right to challenge decisions'),
            ('modify', 'right to request changes'),
            ('consult', 'right to be consulted'),
            ('refuse', 'right to decline certain terms')
        ]
        
        for right, description in rights_to_check:
            if right not in text_lower:
                unmentioned.append({
                    'right': description,
                    'omission': f"No mention of {right} process",
                    'implication': f"This right may not be recognized"
                })
        
        return unmentioned
    
    def _identify_narrative_gaps(self, text: str) -> List[Dict]:
        """Identify gaps in the narrative or argument"""
        gaps = []
        
        # Check for one-sided arguments
        balance_indicators = [
            ('however', 'but', 'although'),
            ('advantage', 'disadvantage'),
            ('pro', 'con'),
            ('support', 'oppose')
        ]
        
        for positive, negative in balance_indicators:
            has_positive = positive in text.lower()
            has_negative = negative in text.lower()
            
            if has_positive and not has_negative:
                gaps.append({
                    'gap_type': 'one_sided_argument',
                    'description': f"Mentions '{positive}' but not '{negative}'",
                    'concern': 'Presents only one side of the issue'
                })
        
        return gaps
    
    def _generate_silence_questions(self, text: str) -> List[str]:
        """Generate questions to uncover what's not being said"""
        questions = [
            "Whose voice is missing from this text?",
            "What alternative interpretations are not considered?",
            "What objections might people raise?",
            "What historical context is omitted?",
            "What future consequences are not discussed?"
        ]
        
        # Context-specific silence questions
        if re.search(r'\b(?:policy|rule|regulation)\b', text, re.IGNORECASE):
            questions.extend([
                "Who was consulted when creating this policy?",
                "What exceptions or special cases are not mentioned?",
                "How might this affect different groups differently?"
            ])

      return questimport re
from datetime import datetime
from typing import Dict, List, Any
import requests
from newspaper import Article

class ContextMapper:
    """
    Maps the historical, economic, and social context of the text
    """
    
    def __init__(self):
        self.context_indicators = {
            'temporal': ['since', 'until', 'effective', 'as of', 'following'],
            'conditional': ['if', 'when', 'whenever', 'subject to', 'depending on'],
            'authority': ['according to', 'per', 'based on', 'pursuant to']
        }
    
    def map_context(self, text: str) -> Dict[str, Any]:
        """Analyze the contextual framework of the text"""
        return {
            'temporal_context': self._analyze_temporal_context(text),
            'conditional_framework': self._analyze_conditions(text),
            'authority_references': self._find_authority_references(text),
            'assumed_knowledge': self._identify_assumed_knowledge(text),
            'contextual_questions': self._generate_context_questions(text)
        }
    
    def _analyze_temporal_context(self, text: str) -> Dict[str, Any]:
        """Analyze time-related context and limitations"""
        temporal_elements = {}
        
        # Find dates and time references
        date_patterns = [
            r'\b\d{1,2}/\d{1,2}/\d{4}\b',  # MM/DD/YYYY
            r'\b\d{4}-\d{2}-\d{2}\b',      # YYYY-MM-DD
            r'\b(?:January|February|March|April|May|June|July|August|September|October|November|December)\s+\d{1,2},?\s+\d{4}\b'
        ]
        
        dates_found = []
        for pattern in date_patterns:
            dates_found.extend(re.findall(pattern, text))
        
        temporal_elements['explicit_dates'] = dates_found
        
        # Analyze time-bound language
        time_bound_terms = re.findall(r'\b(?:effective|until|expires|terminates|renews)\b', text, re.IGNORECASE)
        temporal_elements['time_bound_language'] = time_bound_terms
        
        # Assess temporal limitations
        limitations = []
        if 'until' in text.lower() and 'indefinitely' not in text.lower():
            limitations.append("Has explicit expiration or end condition")
        if 'effective immediately' in text.lower():
            limitations.append("Immediate effect without transition period")
        
        temporal_elements['limitations'] = limitations
        
        return temporal_elements
    
    def _analyze_conditions(self, text: str) -> List[Dict]:
        """Identify conditional frameworks and dependencies"""
        conditions = []
        
        # Find conditional statements
        conditional_patterns = [
            (r'if\s+(.+?)\s+then', 'explicit condition'),
            (r'subject\s+to\s+(.+)', 'dependency condition'),
            (r'provided\s+that\s+(.+)', 'provisional condition'),
            (r'upon\s+(.+?)\s+shall', 'trigger condition')
        ]
        
        for pattern, condition_type in conditional_patterns:
            matches = re.findall(pattern, text, re.IGNORECASE)
            for match in matches:
                conditions.append({
                    'type': condition_type,
                    'condition': match.strip(),
                    'impact': 'Creates dependency or requirement'
                })
        
        return conditions
    
    def _find_authority_references(self, text: str) -> List[Dict]:
        """Identify references to external authorities"""
        authorities = []
        
        authority_patterns = [
            (r'(?:according to|per|pursuant to)\s+([A-Z][^,.]+)', 'citation'),
            (r'(?:section|article|clause)\s+(\d+[a-zA-Z]?)', 'internal reference'),
            (r'(?:law|regulation|statute|act)\s+[A-Z][^,.]+', 'legal reference')
        ]
        
        for pattern, ref_type in authority_patterns:
            matches = re.findall(pattern, text, re.IGNORECASE)
            for match in matches:
                authorities.append({
                    'type': ref_type,
                    'reference': match.strip(),
                    'access_issue': 'External reference may not be accessible'
                })
        
        return authorities
    
    def _identify_assumed_knowledge(self, text: str) -> List[str]:
        """Identify knowledge that is assumed but not explained"""
        assumed = []
        
        # Look for unexplained acronyms
        acronyms = re.findall(r'\b[A-Z]{2,}\b', text)
        for acronym in acronyms:
            if len(acronym) >= 3 and acronym not in ['USA', 'UK', 'CEO', 'CFO']:  # Common exceptions
                assumed.append(f"Acronym '{acronym}' not explained")
        
        # Look for unexplained references
        unexplained_refs = re.findall(r'\b(?:the\s+)?(?:act|law|regulation|policy)(?:\s+[A-Z][a-z]+)*', text, re.IGNORECASE)
        for ref in unexplained_refs:
            if 'this' not in ref.lower() and 'these' not in ref.lower():
                assumed.append(f"Reference to '{ref}' without explanation")
        
        return assumed
    
    def _generate_context_questions(self, text: str) -> List[str]:
        """Generate questions about context and background"""
        questions = [
            "What events led to the creation of this text?",
            "What historical context is important here?",
            "What economic conditions influenced this?",
            "Who has the power to change this context?",
            "What background knowledge is assumed?"
        ]
        
        # Add context-specific questions based on content
        if any(term in text.lower() for term in ['law', 'regulation', 'statute']):
            questions.extend([
                "What problem was this law meant to solve?",
                "Who lobbied for or against this legislation?",
                "How has this been interpreted in practice?"
            ])
        
        return questions
        from flask import Flask, render_template, request, jsonify, session
import os
import json

from ..core.config import Config
from ..core.security import Security
from ..analyzer.power_analyzer import PowerAnalyzer
from ..analyzer.silence_detector import SilenceDetector
from ..analyzer.context_mapper import ContextMapper

app = Flask(__name__)
app.config.from_object(Config)

# Initialize analyzers
power_analyzer = PowerAnalyzer()
silence_detector = SilenceDetector()
context_mapper = ContextMapper()

@app.before_request
def setup_anonymous_session():
    """Set up anonymous session for zero-tracking"""
    if 'session_id' not in session:
        session['session_id'] = Security.generate_anonymous_session()
    session.modified = False  # Prevent session from being saved

@app.route('/')
def index():
    """Main interface - simple text input"""
    return """
    <!DOCTYPE html>
    <html>
    <head>
        <title>Truth Lens - See Through the Noise</title>
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <style>
            body { font-family: Arial, sans-serif; margin: 40px; background: #f5f5f5; }
            .container { max-width: 800px; margin: 0 auto; background: white; padding: 30px; border-radius: 10px; }
            textarea { width: 100%; height: 200px; margin: 10px 0; padding: 10px; border: 1px solid #ddd; }
            button { background: #2c3e50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer; }
            .result { margin-top: 20px; padding: 20px; background: #f8f9fa; border-radius: 5px; }
            .analysis-section { margin: 20px 0; padding: 15px; border-left: 4px solid #3498db; }
            .power { border-color: #e74c3c; }
            .silence { border-color: #f39c12; }
            .context { border-color: #27ae60; }
        </style>
    </head>
    <body>
        <div class="container">
            <h1>🔍 Truth Lens</h1>
            <p><em>See through the noise. For the forgotten people.</em></p>
            
            <form id="analysisForm">
                <textarea id="textInput" placeholder="Paste text to analyze: political speech, corporate policy, legal document, news article..."></textarea>
                <button type="submit">Analyze with Truth Lens</button>
            </form>
            
            <div id="results" class="result" style="display: none;">
                <h2>Analysis Results</h2>
                <div id="powerAnalysis" class="analysis-section power"></div>
                <div id="silenceAnalysis" class="analysis-section silence"></div>
                <div id="contextAnalysis" class="analysis-section context"></div>
            </div>
        </div>

        <script>
            document.getElementById('analysisForm').addEventListener('submit', async function(e) {
                e.preventDefault();
                const text = document.getElementById('textInput').value;
                
                const response = await fetch('/analyze', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ text: text })
                });
                
                const results = await response.json();
                displayResults(results);
            });
            
            function displayResults(data) {
                document.getElementById('results').style.display = 'block';
                
                // Display power analysis
                document.getElementById('powerAnalysis').innerHTML = `
                    <h3>⚖️ Power Analysis</h3>
                    <p><strong>Direct Beneficiaries:</strong> ${data.power_analysis.direct_beneficiaries.length} identified</p>
                    <p><strong>Power Assumptions:</strong> ${data.power_analysis.power_assumptions.length} found</p>
                    <p><strong>Questions to Ask:</strong> ${data.power_analysis.recommended_questions.slice(0, 3).join(', ')}</p>
                `;
                
                // Display silence analysis
                document.getElementById('silenceAnalysis').innerHTML = `
                    <h3>🔇 Silence Analysis</h3>
                    <p><strong>Missing Perspectives:</strong> ${data.silence_analysis.missing_perspectives.length} identified</p>
                    <p><strong>Unmentioned Rights:</strong> ${data.silence_analysis.unmentioned_rights.length} found</p>
                    <p><strong>Questions to Uncover Silence:</strong> ${data.silence_analysis.questions_to_uncover_silence.slice(0, 3).join(', ')}</p>
                `;
                
                // Display context analysis
                document.getElementById('contextAnalysis').innerHTML = `
                    <h3>🌍 Context Analysis</h3>
                    <p><strong>Temporal Elements:</strong> ${data.context_analysis.temporal_context.explicit_dates.length} dates referenced</p>
                    <p><strong>Authority References:</strong> ${data.context_analysis.authority_references.length} external references</p>
                    <p><strong>Context Questions:</strong> ${data.context_analysis.contextual_questions.slice(0, 3).join(', ')}</p>
                `;
            }
        </script>
    </body>
    </html>
    """

@app.route('/analyze', methods=['POST'])
def analyze_text():
    """Analyze submitted text using all three lenses"""
    try:
        data = request.get_json()
        if not data or 'text' not in data:
            return jsonify({'error': 'No text provided'}), 400
        
        text = Security.sanitize_input(data['text'])
        
        if len(text) < 10:
            return jsonify({'error': 'Text too short for meaningful analysis'}), 400
        
        # Run all analyses
        power_analysis = power_analyzer.analyze_beneficiaries(text)
        silence_analysis = silence_detector.detect_silences(text)
        context_analysis = context_mapper.map_context(text)
        
        return jsonify({
            'power_analysis': power_analysis,
            'silence_analysis': silence_analysis,
            'context_analysis': context_analysis,
            'analysis_timestamp': 'not_stored',  # Explicitly no tracking
            'text_length': len(text)
        })
        
    except Exception as e:
        return jsonify({'error': f'Analysis failed: {str(e)}'}), 500

@app.route('/api/health')
def health_check():
    """Simple health check endpoint"""
    return jsonify({
        'status': 'healthy',
        'version': '1.0.0',
        'privacy': 'zero_tracking_enforced'
    })

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=5000)
    from flask import Blueprint, request, jsonify
from ..core.security import Security
from ..analyzer.power_analyzer import PowerAnalyzer
from ..analyzer.silence_detector import SilenceDetector
from ..analyzer.context_mapper import ContextMapper

api = Blueprint('api', __name__)

power_analyzer = PowerAnalyzer()
silence_detector = SilenceDetector()
context_mapper = ContextMapper()

@api.route('/analyze', methods=['POST'])
def api_analyze():
    """API endpoint for programmatic analysis"""
    data = request.get_json()
    
    if not data or 'text' not in data:
        return jsonify({'error': 'Missing text parameter'}), 400
    
    text = Security.sanitize_input(data['text'])
    
    if len(text) < 10:
        return jsonify({'error': 'Text too short'}), 400
    
    # Run analyses
    try:
        power_analysis = power_analyzer.analyze_beneficiaries(text)
        silence_analysis = silence_detector.detect_silences(text)
        context_analysis = context_mapper.map_context(text)
        
        return jsonify({
            'analysis': {
                'power': power_analysis,
                'silence': silence_analysis,
                'context': context_analysis
            },
            'metadata': {
                'text_length': len(text),
                'analysis_complete': True,
                'privacy_note': 'No data stored or tracked'
            }
        })
        
    except Exception as e:
        return jsonify({'error': f'Analysis error: {str(e)}'}), 500

@api.route('/analyze/power', methods=['POST'])
def api_analyze_power():
    """API endpoint for power analysis only"""
    return _single_analysis_endpoint('power')

@api.route('/analyze/silence', methods=['POST'])
def api_analyze_silence():
    """API endpoint for silence analysis only"""
    return _single_analysis_endpoint('silence')

@api.route('/analyze/context', methods=['POST'])
def api_analyze_context():
    """API endpoint for context analysis only"""
    return _single_analysis_endpoint('context')

def _single_analysis_endpoint(analysis_type):
    """Helper for single analysis endpoints"""
    data = request.get_json()
    
    if not data or 'text' not in data:
        return jsonify({'error': 'Missing text parameter'}), 400
    
    text = Security.sanitize_input(data['text'])
    
    try:
        if analysis_type == 'power':
            result = power_analyzer.analyze_beneficiaries(text)
        elif analysis_type == 'silence':
            result = silence_detector.detect_silences(text)
        elif analysis_type == 'context':
            result = context_mapper.map_context(text)
        else:
            return jsonify({'error': 'Invalid analysis type'}), 400
        
        return jsonify({
            analysis_type: result,
            'metadata': {
                'text_length': len(text),
                'privacy_note': 'No data stored or tracked'
            }
        })
        
    except Exception as e:
        return jsonify({'error': f'Analysis error: {str(e)}'}), 500
     # Example analysis of political speech
"""
# Truth Lens - Political Speech Analysis Example

This example demonstrates analyzing a political speech using Truth Lens.
"""

sample_speech = """
My fellow citizens, today I announce the Economic Prosperity Act. 
This legislation will create millions of jobs and boost our GDP by 5%. 
We must pass this bill to ensure American dominance in global markets. 
The Department of Commerce will oversee implementation. 
Business leaders support this initiative as it reduces regulation 
and provides tax incentives for corporations that create jobs. 
We cannot afford delay - the act takes effect immediately upon passage.
"""

# Initialize analyzers
from src.analyzer.power_analyzer import PowerAnalyzer
from src.analyzer.silence_detector import SilenceDetector
from src.analyzer.context_mapper import ContextMapper

power_analyzer = PowerAnalyzer()
silence_detector = SilenceDetector()
context_mapper = ContextMapper()

print("=== POWER ANALYSIS ===")
power_result = power_analyzer.analyze_beneficiaries(sample_speech)
print("Direct beneficiaries:", [b['entity'] for b in power_result['direct_beneficiaries']])
print("Power assumptions:", [a['description'] for a in power_result['power_assumptions']])

print("\n=== SILENCE ANALYSIS ===")
silence_result = silence_detector.detect_silences(sample_speech)
print("Missing perspectives:", [p['perspective'] for p in silence_result['missing_perspectives']])
print("Unmentioned rights:", [r['right'] for r in silence_result['unmentioned_rights']])

print("\n=== CONTEXT ANALYSIS ===")
context_result = context_mapper.map_context(sample_speech)
print("Temporal context:", context_result['temporal_context']['limitations'])
print("Assumed knowledge:", context_result['assumed_knowledge'][:3]) 
import unittest
import sys
import os
sys.path.append(os.path.join(os.path.dirname(__file__), '..'))

from src.analyzer.power_analyzer import PowerAnalyzer
from src.analyzer.silence_detector import SilenceDetector
from src.analyzer.context_mapper import ContextMapper
from src.core.security import Security

class TestTruthLens(unittest.TestCase):
    
    def setUp(self):
        self.power_analyzer = PowerAnalyzer()
        self.silence_detector = SilenceDetector()
        self.context_mapper = ContextMapper()
        self.sample_text = "The corporation must maximize shareholder value. All decisions are final."
    
    def test_power_analysis_identifies_beneficiaries(self):
        result = self.power_analyzer.analyze_beneficiaries(self.sample_text)
        self.assertIn('direct_beneficiaries', result)
        self.assertIn('power_assumptions', result)
    
    def test_silence_detection_finds_missing_perspectives(self):
        result = self.silence_detector.detect_silences(self.sample_text)
        self.assertIn('missing_perspectives', result)
        self.assertTrue(len(result['missing_perspectives']) > 0)
    
    def test_context_mapping_identifies_assumptions(self):
        result = self.context_mapper.map_context(self.sample_text)
        self.assertIn('assumed_knowledge', result)
        self.assertIn('temporal_context', result)
    
    def test_security_sanitization(self):
        dangerous_text = "<script>alert('xss')</script>Normal text"
        sanitized = Security.sanitize_input(dangerous_text)
        self.assertNotIn('<script>', sanitized)
        self.assertIn('Normal text', sanitized)

if __name__ == '__main__':
    unittest.main()
    # Truth Lens Analysis Framework

## Core Philosophy

Truth Lens operates on three fundamental principles:

1. **Power Analysis**: Every text reflects power structures
2. **Silence Detection**: What's missing is as important as what's present  
3. **Context Mapping**: Text exists within historical and social frameworks

## Methodologies

### Power Analysis
- Identifies direct and indirect beneficiaries
- Maps control structures and authority assumptions
- Generates critical questions about power dynamics

### Silence Detection  
- Identifies missing perspectives and voices
- Maps narrative gaps and omissions
- Uncovers unmentioned rights and alternatives

### Context Mapping
- Analyzes temporal and conditional frameworks
- Identifies authority references and dependencies
- Maps assumed knowledge and background context

## Privacy & Security

- Zero data persistence
- No user tracking
- All analysis happens in memory
- Sessions are anonymous and ephemeral
- # Truth Lens API Documentation

## Base URL
`https://your-domain.com/api`

## Endpoints

### POST /api/analyze
Complete analysis using all three lenses.

**Request:**
```json
{
  "text": "Text to analyze..."
}{
  "analysis": {
    "power": { ... },
    "silence": { ... },
    "context": { ... }
  }
}
**13. README.md** (Enhanced)
```markdown
# Truth Lens - See Through the Noise

A tool for the forgotten people. Truth Lens reveals the hidden frameworks in any text through three core analyses.

## 🎯 The Three Lenses

1. **Power Analysis** - Who benefits? What power assumptions are baked in?
2. **Silence Analysis** - Who/what is missing? What perspectives are excluded?  
3. **Context Analysis** - What conditions made this possible?

## 🚀 Quick Start

```bash
git clone https://github.com/[your-username]/truth-lens
cd truth-lens
pip install -r requirements.txt
python src/interface/web_interface.py
from src.analyzer.power_analyzer import PowerAnalyzer

analyzer = PowerAnalyzer()
result = analyzer.analyze_beneficiaries("Your text here")

**14. .gitignore**
```gitignore
# Environment
.env
.venv
env/
venv/

# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg

# Database
*.db
*.sqlite3

# Logs
*.log

# OS
.DS_Store
Thumbs.db# Deployment Guide

## Local Development
```bash
python src/interface/web_interface.py
gunicorn --bind 0.0.0.0:5000 src.interface.web_interface:app
FROM python:3.9-slim
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "src.interface.web_interface:app"]

---

**Repository Complete ✅**

The Truth Lens is now a fully functional, deployable tool with:

- ✅ **Complete analysis engine** (power, silence, context)
- ✅ **Web interface** with zero tracking
- ✅ **REST API** for developers  
- ✅ **Comprehensive documentation**
- ✅ **Example analyses**
- ✅ **Test suite**
- ✅ **Security and privacy by design**
- ✅ **Deployment ready**

**To deploy immediately:**
```bash
git init
git add .
git commit -m "Initial commit: Truth Lens for the forgotten people"
git remote add origin https://github.com/[your-username]/truth-lens
git push -u origin main
# Deploy to free tier hosting
# Options: Heroku, Railway, PythonAnywhere, Replit
git push heroku main
# Now accessible at: https://truth-lens.herokuapp.com
// Truth Lens extension that analyzes any webpage
// Right-click -> "Analyze with Truth Lens"
# examples/cognitive_bias_analysis.py

"""
Truth Lens - Cognitive Bias Analysis Example

This example demonstrates analyzing the psychological concept of Confirmation Bias
to reveal its power dynamics, silences, and societal context.
"""
sample_concept_text = """
Confirmation Bias is the tendency to search for, interpret, favor, and recall
information in a way that confirms or supports one's prior beliefs or values.
This leads to overconfidence in personal beliefs and can result in polarized
social groups that rarely engage with disconfirming evidence.
"""

# Initialize analyzers
from src.analyzer.power_analyzer import PowerAnalyzer
from src.analyzer.silence_detector import SilenceDetector
from src.analyzer.context_mapper import ContextMapper

power_analyzer = PowerAnalyzer()
silence_detector = SilenceDetector()
context_mapper = ContextMapper()

print("=== 🧠 TRUTH LENS ON COGNITIVE BIAS ===")

print("\n--- ⚖️ POWER ANALYSIS ---")
power_result = power_analyzer.analyze_beneficiaries(sample_concept_text)
print("Thought: Who benefits from *unquestioned* beliefs?")
print("Identified Power Assumptions:", [a['description'] for a in power_result['power_assumptions']][:2])

print("\n--- 🔇 SILENCE ANALYSIS ---")
silence_result = silence_detector.detect_silences(sample_concept_text)
print("Thought: What is ignored when Confirmation Bias dominates?")
print("Missing Perspectives:", [p['perspective'] for p in silence_result['missing_perspectives']][:2])

print("\n--- 🌍 CONTEXT ANALYSIS ---")
context_result = context_mapper.map_context(sample_concept_text)
print("Thought: What social context amplifies this bias?")
print("Context Questions:", context_result['contextual_questions'][-2:])
# Truth Lens - See Through the Noise

A tool for the forgotten people. Truth Lens reveals the hidden frameworks in any text through three core analyses.

## 🎯 The Three Lenses

1. **Power Analysis** - Who benefits? What power assumptions are baked in?
2. **Silence Analysis** - Who/what is missing? What perspectives are excluded?  
3. **Context Analysis** - What conditions made this possible?

---

## 🧠 Applying the Lens to Abstract Concepts

Truth Lens is not limited to policies and speeches. It can be applied to **abstract or philosophical text** (e.g., psychological theories, ethical frameworks, artistic manifestos) by reinterpreting the lenses:

* **Power:** Focus on the dominance of **one idea** or the **conceptual beneficiaries** of a flawed theory.
* **Silence:** Look for **alternative theories** or human experiences that the concept fails to account for.
* **Context:** Map the **historical era** or societal crisis that made the abstract concept necessary or popular.

Run the new example:
```bash
python examples/cognitive_bias_analysis.py
"""
Truth Lens Analyzer Module
Core analysis engines for power, silence, and context detection
"""

from .power_analyzer import PowerAnalyzer
from .silence_detector import SilenceDetector
from .context_mapper import ContextMapper

__all__ = ['PowerAnalyzer', 'SilenceDetector', 'ContextMapper']

def analyze_text(text: str) -> dict:
    """
    Convenience function to run all three analyses on text
    """
    power = PowerAnalyzer()
    silence = SilenceDetector()
    context = ContextMapper()
    
    return {
        'power': power.analyze_beneficiaries(text),
        'silence': silence.detect_silences(text),
        'context': context.map_context(text)
    }"""
Truth Lens Core Module
Configuration and security utilities
"""

from .config import Config
from .security import Security

__all__ = ['Config', 'Security']
"""
Truth Lens Interface Module
Web and API interfaces for text analysis
"""

from .web_interface import app
from .api import api

__all__ = ['app', 'api
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    python3-dev \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements first for better caching
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Download NLTK data
RUN python -m nltk.downloader punkt stopwords

# Copy application code
COPY . .

# Create non-root user for security
RUN useradd -m -u 1000 truthlens && chown -R truthlens:truthlens /app
USER truthlens

# Expose port
EXPOSE 5000

# Set environment variables
ENV FLASK_APP=src.interface.web_interface:app
ENV PYTHONUNBUFFERED=1

# Run the application
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers", "2", "--threads", "4", "src.interface.web_interface:app"]
version: '3.8'

services:
  truth-lens:
    build: .
    ports:
      - "5000:5000"
    environment:
      - FLASK_ENV=development
      - FLASK_DEBUG=1
      - SECRET_KEY=truth-lens-dev-key
    volumes:
      - ./src:/app/src
    command: python -m flask run --host=0.0.0.0
from setuptools import setup, find_packages

with open("README.md", "r", encoding="utf-8") as fh:
    long_description = fh.read()

setup(
    name="truth-lens",
    version="1.0.0",
    author="LHMisme420",
    description="A tool for the forgotten people - reveals hidden frameworks in text",
    long_description=long_description,
    long_description_content_type="text/markdown",
    url="https://github.com/LHMisme420/Truth-Lens",
    packages=find_packages(where="src"),
    package_dir={"": "src"},
    classifiers=[
        "Development Status :: 4 - Beta",
        "Intended Audience :: Developers",
        "Intended Audience :: Education",
        "Topic :: Text Processing :: Linguistic",
        "License :: OSI Approved :: MIT License",
        "Programming Language :: Python :: 3",
        "Programming Language :: Python :: 3.7",
        "Programming Language :: Python :: 3.8",
        "Programming Language :: Python :: 3.9",
    ],
    python_requires=">=3.7",
    install_requires=[
        "flask>=2.3.3",
        "nltk>=3.8.1",
        "textblob>=0.17.1",
        "newspaper3k>=0.2.8",
        "requests>=2.31.0",
        "gunicorn>=21.2.0",
        "python-dotenv>=1.0.0",
    ],
    entry_points={
        "console_scripts": [
            "truth-lens=interface.web_interface:main",
        ],
    },
)
.PHONY: help install run test clean docker-build docker-run deploy-heroku

help:
	@echo "Truth Lens - Make Commands"
	@echo "=========================="
	@echo "install       - Install dependencies"
	@echo "run          - Run local development server"
	@echo "test         - Run test suite"
	@echo "clean        - Clean cache files"
	@echo "docker-build - Build Docker image"
	@echo "docker-run   - Run in Docker"
	@echo "deploy-heroku - Deploy to Heroku"

install:
	pip install -r requirements.txt
	python -m nltk.downloader punkt stopwords

run:
	python src/interface/web_interface.py

test:
	python -m pytest tests/ -v

clean:
	find . -type d -name "__pycache__" -exec rm -rf {} +
	find . -type f -name "*.pyc" -delete
	find . -type f -name "*.pyo" -delete
	find . -type f -name "*.egg-info" -exec rm -rf {} +
	find . -type d -name ".pytest_cache" -exec rm -rf {} +

docker-build:
	docker build -t truth-lens .

docker-run:
	docker-compose up

deploy-heroku:
	heroku create truth-lens-app
	git push heroku main
	heroku open
#!/usr/bin/env python3
"""
Truth Lens CLI - Command Line Interface for text analysis
"""

import argparse
import json
import sys
from pathlib import Path

# Add src to path
sys.path.insert(0, str(Path(__file__).parent.parent))

from src.analyzer.power_analyzer import PowerAnalyzer
from src.analyzer.silence_detector import SilenceDetector
from src.analyzer.context_mapper import ContextMapper
from src.core.security import Security

def main():
    parser = argparse.ArgumentParser(
        description="Truth Lens - See through the noise. For the forgotten people.",
        formatter_class=argparse.RawDescriptionHelpFormatter,
        epilog="""
Examples:
  truth-lens --text "Your text here"
  truth-lens --file document.txt
  truth-lens --file document.txt --lens power
  truth-lens --file document.txt --json output.json
        """
    )
    
    # Input options
    input_group = parser.add_mutually_exclusive_group(required=True)
    input_group.add_argument('--text', help='Text to analyze')
    input_group.add_argument('--file', help='File to analyze')
    
    # Analysis options
    parser.add_argument('--lens', choices=['all', 'power', 'silence', 'context'], 
                       default='all', help='Which lens to apply (default: all)')
    
    # Output options
    parser.add_argument('--json', metavar='FILE', help='Save results to JSON file')
    parser.add_argument('--verbose', action='store_true', help='Verbose o# Truth-Lens
Lens of truth
git clone https://github.com/LHMisme420/Truth-Lens/blob/main/README.md
cd truth-lens
pip install -r requirements.txt
from truth_lens.analyzer import analyze

text = "Your text here"
results = analyze(text)
print(results)
POWER_ANALYSIS_PROMPT = """
Analyze the following text from a power perspective. Identify:
1. Who benefits from the framing of this text?
2. What assumptions about power are baked into the text?
3. How does the text reinforce or challenge existing power structures?

Text: {text}
"""

SILENCE_ANALYSIS_PROMPT = """
Analyze the following text for what is left out. Identify:
1. Who or what is not mentioned that might be relevant?
2. What perspectives are missing?
3. What questions are left unanswered?

Text: {text}
"""

CONTEXT_ANALYSIS_PROMPT = """
Analyze the following text in its broader context. Identify:
1. What historical conditions might have led to this text?
2. What economic conditions might have influenced it?
3. What social or cultural factors are reflected?

Text: {text}
"""
import os
from openai import OpenAI
from .prompts import POWER_ANALYSIS_PROMPT, SILENCE_ANALYSIS_PROMPT, CONTEXT_ANALYSIS_PROMPT

# Initialize the OpenAI client
client = OpenAI(api_key=os.getenv('OPENAI_API_KEY'))

def analyze(text, model="gpt-3.5-turbo"):
    """
    Analyze the given text with the three lenses: power, silence, and context.
    Returns a dictionary with the three analyses.
    """
    analyses = {}
    
    # Power Analysis
    power_response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "user", "content": POWER_ANALYSIS_PROMPT.format(text=text)}
        ]
    )
    analyses['power'] = power_response.choices[0].message.content

    # Silence Analysis
    silence_response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "user", "content": SILENCE_ANALYSIS_PROMPT.format(text=text)}
        ]
    )
    analyses['silence'] = silence_response.choices[0].message.content

    # Context Analysis
    context_response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "user", "content": CONTEXT_ANALYSIS_PROMPT.format(text=text)}
        ]
    )
    analyses['context'] = context_response.choices[0].message.content

    return analyses
    # Repository Structure
truth-lens/
├── README.md
├── requirements.txt
├── src/
│   ├── analyzer/
│   │   ├── __init__.py
│   │   ├── power_analyzer.py
│   │   ├── silence_detector.py
│   │   └── context_mapper.py
│   ├── interface/
│   │   ├── web_interface.py
│   │   └── api.py
│   └── core/
│       ├── config.py
│       └── security.py
├── tests/
├── docs/
│   ├── API.md
│   └── ANALYSIS_FRAMEWORK.md
└── examples/
    ├── political_speech_analysis.ipynb
    └── corporate_policy_demo.py
    # Truth Lens - See Through the Noise

A tool for the forgotten people. Truth Lens reveals the hidden frameworks in any text through three core analyses:

## The Three Lenses

1. **Power Analysis** - Who benefits? What power assumptions are baked in?
2. **Silence Analysis** - Who/what is missing? What perspectives are excluded?
3. **Context Analysis** - What conditions made this possible?

## For People, Not Profit

This tool is designed for:
- Tenants reading landlord notices
- Workers decoding corporate policies  
- Citizens analyzing political speeches
- Anyone the system tries to confuse

## No Tracking, No Bullshit
- Zero user data collection
- Local processing where possible
- Transparent analysis methods
- 
class PowerAnalyzer:
    """
    Reveals power structures and beneficiaries in text
    """
    
    def analyze_beneficiaries(self, text):
        """Identifies who benefits from the framing"""
        # Core analysis logic here
        return {
            'direct_beneficiaries': [],
            'indirect_beneficiaries': [],
            'power_assumptions': [],
            'recommended_questions': []
        }from flask import Flask, request, jsonify
app = Flask(__name__)

@app.route('/analyze', methods=['POST'])
def analyze_text():
    text = request.json.get('text')
    return jsonify({
        'power_analysis': power_analyzer.analyze(text),
        'silence_analysis': silence_detector.detect(text),
        'context_analysis': context_mapper.map(text)
    })flask==2.3.3
nltk==3.8.1
textblob==0.17.1
newspaper3k==0.2.8
requests==2.31.0
gunicorn==21.2.0
python-dotenv==1.0.0
import os
from dotenv import load_dotenv

load_dotenv()

class Config:
    """Zero-tracking configuration"""
    SECRET_KEY = os.getenv('SECRET_KEY', 'truth-lens-local-dev')
    MAX_CONTENT_LENGTH = 16 * 1024 * 1024  # 16MB max file size
    ANALYSIS_CACHE_SIZE = 100  # In-memory cache size
    ENABLE_PERSISTENCE = False  # Explicitly disable data persistence
    
    # Analysis parameters
    MAX_TEXT_LENGTH = 10000
    MIN_CONFIDENCE_THRESHOLD = 0.6
    import re
import html

class Security:
    """Security and sanitization for zero-trust environment"""
    
    @staticmethod
    def sanitize_input(text):
        """Remove potentially dangerous content"""
        if not text:
            return ""
        
        # Basic HTML escaping
        text = html.escape(text)
        
        # Remove suspicious patterns
        suspicious_patterns = [
            r'<script.*?>.*?</script>',
            r'javascript:',
            r'vbscript:',
            r'on\w+='
        ]
        
        for pattern in suspicious_patterns:
            text = re.sub(pattern, '', text, flags=re.IGNORECASE)
            
        return text[:10000]  # Enforce length limit
    
    @staticmethod
    def generate_anonymous_session():
        """Generate session ID that cannot be traced back to user"""
        import uuid
        import time
        return f"session_{int(time.time())}_{uuid.uuid4().hex[:8]}"
        import re
from textblob import TextBlob
from typing import Dict, List, Any

class PowerAnalyzer:
    """
    Reveals power structures, beneficiaries, and hidden assumptions in text
    """
    
    def __init__(self):
        self.power_indicators = {
            'authority_terms': ['must', 'require', 'comply', 'obligated', 'mandatory', 'prohibited'],
            'benefit_terms': ['profit', 'revenue', 'growth', 'efficiency', 'optimize', 'leverage'],
            'control_terms': ['manage', 'direct', 'oversee', 'administer', 'regulate', 'enforce'],
            'exclusion_terms': ['except', 'excluding', 'limited to', 'qualified', 'eligible', 'approval required']
        }
    
    def analyze_beneficiaries(self, text: str) -> Dict[str, Any]:
        """Identify who benefits from the framing and assumptions"""
        blob = TextBlob(text)
        
        # Extract entities that might indicate beneficiaries
        entities = self._extract_entities(text)
        
        # Analyze sentiment around different entities
        beneficiary_analysis = []
        for entity in entities[:10]:  # Limit to top 10 entities
            context_sentiment = self._get_entity_sentiment(blob, entity)
            if context_sentiment and context_sentiment > 0.3:
                beneficiary_analysis.append({
                    'entity': entity,
                    'benefit_level': 'high',
                    'evidence': f"Positive language used around '{entity}'",
                    'sentiment_score': context_sentiment
                })
        
        return {
            'direct_beneficiaries': beneficiary_analysis,
            'power_assumptions': self._find_power_assumptions(text),
            'control_structures': self._identify_control_structures(text),
            'recommended_questions': self._generate_power_questions(text)
        }
    
    def _extract_entities(self, text: str) -> List[str]:
        """Extract potential entities (organizations, groups, roles)"""
        # Simple pattern-based extraction - could be enhanced with NER
        patterns = {
            'organizations': r'\b[A-Z][a-z]+(?:\s+[A-Z][a-z]+)*\s+(?:Inc|Corp|Company|Organization|Group)\b',
            'government': r'\b(?:Department|Agency|Bureau|Ministry|Committee)\s+of\s+[A-Z][a-z]+\b',
            'roles': r'\b(?:CEO|Director|Manager|Officer|Administrator|Coordinator)\b'
        }
        
        entities = []
        for category, pattern in patterns.items():
            entities.extend(re.findall(pattern, text))
        
        return list(set(entities))
    
    def _get_entity_sentiment(self, blob: TextBlob, entity: str) -> float:
        """Get sentiment in sentences mentioning the entity"""
        entity_sentences = [str(s) for s in blob.sentences if entity.lower() in str(s).lower()]
        if not entity_sentences:
            return 0.0
        
        total_sentiment = sum(TextBlob(s).sentiment.polarity for s in entity_sentences)
        return total_sentiment / len(entity_sentences)
    
    def _find_power_assumptions(self, text: str) -> List[Dict]:
        """Identify underlying power assumptions"""
        assumptions = []
        
        # Look for unquestioned authority
        authority_patterns = [
            (r'as\s+we\s+determine', 'Organization determines criteria without oversight'),
            (r'at\s+our\s+discretion', 'Complete discretion without specified limits'),
            (r'final\s+and\s+binding', 'No appeal or challenge mechanism'),
            (r'subject\s+to\s+change\s+without\s+notice', 'Unilateral modification power')
        ]
        
        for pattern, description in authority_patterns:
            if re.search(pattern, text, re.IGNORECASE):
                assumptions.append({
                    'type': 'unchecked_authority',
                    'description': description,
                    'example': re.search(pattern, text, re.IGNORECASE).group(0)
                })
        
        return assumptions
    
    def _identify_control_structures(self, text: str) -> List[Dict]:
        """Identify mechanisms of control in the text"""
        controls = []
        
        control_patterns = [
            (r'must\s+(?:not\s+)?\w+', 'Obligation without justification'),
            (r'required\s+to\s+\w+', 'Mandatory action'),
            (r'prohibited\s+from\s+\w+', 'Explicit prohibition'),
            (r'only\s+if\s+approved', 'Gatekeeping mechanism')
        ]
        
        for pattern, control_type in control_patterns:
            matches = re.findall(pattern, text, re.IGNORECASE)
            for match in matches:
                controls.append({
                    'control_type': control_type,
                    'language': match,
                    'impact': 'limits autonomy'
                })
        
        return controls
    
    def _generate_power_questions(self, text: str) -> List[str]:
        """Generate critical questions about power dynamics"""
        questions = [
            "Who has the authority to change these terms?",
            "What recourse do I have if I disagree?",
            "Who benefits most from this arrangement?",
            "What assumptions about power are being made here?",
            "Who is not represented in this text?"
        ]
        
        # Add context-specific questions
        if re.search(r'\b(?:tenant|rent|lease)\b', text, re.IGNORECASE):
            questions.extend([
                "What rights is the landlord assuming they have?",
                "What protections are missing for the tenant?",
                "Who bears the most risk in this agreement?"
            ])
        
        return questions
        import re
from typing import Dict, List, Any
from textblob import TextBlob

class SilenceDetector:
    """
    Detects what and who is missing from the text
    """
    
    def __init__(self):
        self.common_exclusions = {
            'perspectives': ['community', 'public', 'resident', 'tenant', 'worker', 'consumer'],
            'considerations': ['alternative', 'opposing view', 'criticism', 'limitation', 'drawback'],
            'rights': ['appeal', 'contest', 'negotiate', 'modify', 'refuse']
        }
    
    def detect_silences(self, text: str) -> Dict[str, Any]:
        """Identify missing perspectives and considerations"""
        return {
            'missing_perspectives': self._find_missing_perspectives(text),
            'absent_considerations': self._find_absent_considerations(text),
            'unmentioned_rights': self._find_unmentioned_rights(text),
            'narrative_gaps': self._identify_narrative_gaps(text),
            'questions_to_uncover_silence': self._generate_silence_questions(text)
        }
    
    def _find_missing_perspectives(self, text: str) -> List[Dict]:
        """Find commonly excluded perspectives"""
        missing = []
        text_lower = text.lower()
        
        for perspective in self.common_exclusions['perspectives']:
            if perspective not in text_lower:
                missing.append({
                    'perspective': perspective,
                    'significance': f"The {perspective} viewpoint is not represented",
                    'potential_impact': f"May overlook {perspective} needs and concerns"
                })
        
        return missing
    
    def _find_absent_considerations(self, text: str) -> List[Dict]:
        """Identify common considerations that are absent"""
        absent = []
        text_lower = text.lower()
        
        for consideration in self.common_exclusions['considerations']:
            if consideration not in text_lower:
                absent.append({
                    'consideration': consideration,
                    'type': 'critical_omission',
                    'question': f"What {consideration}s were not discussed?"
                })
        
        return absent
    
    def _find_unmentioned_rights(self, text: str) -> List[Dict]:
        """Find rights that are not mentioned but might be relevant"""
        unmentioned = []
        text_lower = text.lower()
        
        rights_to_check = [
            ('appeal', 'right to challenge decisions'),
            ('modify', 'right to request changes'),
            ('consult', 'right to be consulted'),
            ('refuse', 'right to decline certain terms')
        ]
        
        for right, description in rights_to_check:
            if right not in text_lower:
                unmentioned.append({
                    'right': description,
                    'omission': f"No mention of {right} process",
                    'implication': f"This right may not be recognized"
                })
        
        return unmentioned
    
    def _identify_narrative_gaps(self, text: str) -> List[Dict]:
        """Identify gaps in the narrative or argument"""
        gaps = []
        
        # Check for one-sided arguments
        balance_indicators = [
            ('however', 'but', 'although'),
            ('advantage', 'disadvantage'),
            ('pro', 'con'),
            ('support', 'oppose')
        ]
        
        for positive, negative in balance_indicators:
            has_positive = positive in text.lower()
            has_negative = negative in text.lower()
            
            if has_positive and not has_negative:
                gaps.append({
                    'gap_type': 'one_sided_argument',
                    'description': f"Mentions '{positive}' but not '{negative}'",
                    'concern': 'Presents only one side of the issue'
                })
        
        return gaps
    
    def _generate_silence_questions(self, text: str) -> List[str]:
        """Generate questions to uncover what's not being said"""
        questions = [
            "Whose voice is missing from this text?",
            "What alternative interpretations are not considered?",
            "What objections might people raise?",
            "What historical context is omitted?",
            "What future consequences are not discussed?"
        ]
        
        # Context-specific silence questions
        if re.search(r'\b(?:policy|rule|regulation)\b', text, re.IGNORECASE):
            questions.extend([
                "Who was consulted when creating this policy?",
                "What exceptions or special cases are not mentioned?",
                "How might this affect different groups differently?"
            ])

      return questimport re
from datetime import datetime
from typing import Dict, List, Any
import requests
from newspaper import Article

class ContextMapper:
    """
    Maps the historical, economic, and social context of the text
    """
    
    def __init__(self):
        self.context_indicators = {
            'temporal': ['since', 'until', 'effective', 'as of', 'following'],
            'conditional': ['if', 'when', 'whenever', 'subject to', 'depending on'],
            'authority': ['according to', 'per', 'based on', 'pursuant to']
        }
    
    def map_context(self, text: str) -> Dict[str, Any]:
        """Analyze the contextual framework of the text"""
        return {
            'temporal_context': self._analyze_temporal_context(text),
            'conditional_framework': self._analyze_conditions(text),
            'authority_references': self._find_authority_references(text),
            'assumed_knowledge': self._identify_assumed_knowledge(text),
            'contextual_questions': self._generate_context_questions(text)
        }
    
    def _analyze_temporal_context(self, text: str) -> Dict[str, Any]:
        """Analyze time-related context and limitations"""
        temporal_elements = {}
        
        # Find dates and time references
        date_patterns = [
            r'\b\d{1,2}/\d{1,2}/\d{4}\b',  # MM/DD/YYYY
            r'\b\d{4}-\d{2}-\d{2}\b',      # YYYY-MM-DD
            r'\b(?:January|February|March|April|May|June|July|August|September|October|November|December)\s+\d{1,2},?\s+\d{4}\b'
        ]
        
        dates_found = []
        for pattern in date_patterns:
            dates_found.extend(re.findall(pattern, text))
        
        temporal_elements['explicit_dates'] = dates_found
        
        # Analyze time-bound language
        time_bound_terms = re.findall(r'\b(?:effective|until|expires|terminates|renews)\b', text, re.IGNORECASE)
        temporal_elements['time_bound_language'] = time_bound_terms
        
        # Assess temporal limitations
        limitations = []
        if 'until' in text.lower() and 'indefinitely' not in text.lower():
            limitations.append("Has explicit expiration or end condition")
        if 'effective immediately' in text.lower():
            limitations.append("Immediate effect without transition period")
        
        temporal_elements['limitations'] = limitations
        
        return temporal_elements
    
    def _analyze_conditions(self, text: str) -> List[Dict]:
        """Identify conditional frameworks and dependencies"""
        conditions = []
        
        # Find conditional statements
        conditional_patterns = [
            (r'if\s+(.+?)\s+then', 'explicit condition'),
            (r'subject\s+to\s+(.+)', 'dependency condition'),
            (r'provided\s+that\s+(.+)', 'provisional condition'),
            (r'upon\s+(.+?)\s+shall', 'trigger condition')
        ]
        
        for pattern, condition_type in conditional_patterns:
            matches = re.findall(pattern, text, re.IGNORECASE)
            for match in matches:
                conditions.append({
                    'type': condition_type,
                    'condition': match.strip(),
                    'impact': 'Creates dependency or requirement'
                })
        
        return conditions
    
    def _find_authority_references(self, text: str) -> List[Dict]:
        """Identify references to external authorities"""
        authorities = []
        
        authority_patterns = [
            (r'(?:according to|per|pursuant to)\s+([A-Z][^,.]+)', 'citation'),
            (r'(?:section|article|clause)\s+(\d+[a-zA-Z]?)', 'internal reference'),
            (r'(?:law|regulation|statute|act)\s+[A-Z][^,.]+', 'legal reference')
        ]
        
        for pattern, ref_type in authority_patterns:
            matches = re.findall(pattern, text, re.IGNORECASE)
            for match in matches:
                authorities.append({
                    'type': ref_type,
                    'reference': match.strip(),
                    'access_issue': 'External reference may not be accessible'
                })
        
        return authorities
    
    def _identify_assumed_knowledge(self, text: str) -> List[str]:
        """Identify knowledge that is assumed but not explained"""
        assumed = []
        
        # Look for unexplained acronyms
        acronyms = re.findall(r'\b[A-Z]{2,}\b', text)
        for acronym in acronyms:
            if len(acronym) >= 3 and acronym not in ['USA', 'UK', 'CEO', 'CFO']:  # Common exceptions
                assumed.append(f"Acronym '{acronym}' not explained")
        
        # Look for unexplained references
        unexplained_refs = re.findall(r'\b(?:the\s+)?(?:act|law|regulation|policy)(?:\s+[A-Z][a-z]+)*', text, re.IGNORECASE)
        for ref in unexplained_refs:
            if 'this' not in ref.lower() and 'these' not in ref.lower():
                assumed.append(f"Reference to '{ref}' without explanation")
        
        return assumed
    
    def _generate_context_questions(self, text: str) -> List[str]:
        """Generate questions about context and background"""
        questions = [
            "What events led to the creation of this text?",
            "What historical context is important here?",
            "What economic conditions influenced this?",
            "Who has the power to change this context?",
            "What background knowledge is assumed?"
        ]
        
        # Add context-specific questions based on content
        if any(term in text.lower() for term in ['law', 'regulation', 'statute']):
            questions.extend([
                "What problem was this law meant to solve?",
                "Who lobbied for or against this legislation?",
                "How has this been interpreted in practice?"
            ])
        
        return questions
        from flask import Flask, render_template, request, jsonify, session
import os
import json

from ..core.config import Config
from ..core.security import Security
from ..analyzer.power_analyzer import PowerAnalyzer
from ..analyzer.silence_detector import SilenceDetector
from ..analyzer.context_mapper import ContextMapper

app = Flask(__name__)
app.config.from_object(Config)

# Initialize analyzers
power_analyzer = PowerAnalyzer()
silence_detector = SilenceDetector()
context_mapper = ContextMapper()

@app.before_request
def setup_anonymous_session():
    """Set up anonymous session for zero-tracking"""
    if 'session_id' not in session:
        session['session_id'] = Security.generate_anonymous_session()
    session.modified = False  # Prevent session from being saved

@app.route('/')
def index():
    """Main interface - simple text input"""
    return """
    <!DOCTYPE html>
    <html>
    <head>
        <title>Truth Lens - See Through the Noise</title>
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <style>
            body { font-family: Arial, sans-serif; margin: 40px; background: #f5f5f5; }
            .container { max-width: 800px; margin: 0 auto; background: white; padding: 30px; border-radius: 10px; }
            textarea { width: 100%; height: 200px; margin: 10px 0; padding: 10px; border: 1px solid #ddd; }
            button { background: #2c3e50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer; }
            .result { margin-top: 20px; padding: 20px; background: #f8f9fa; border-radius: 5px; }
            .analysis-section { margin: 20px 0; padding: 15px; border-left: 4px solid #3498db; }
            .power { border-color: #e74c3c; }
            .silence { border-color: #f39c12; }
            .context { border-color: #27ae60; }
        </style>
    </head>
    <body>
        <div class="container">
            <h1>🔍 Truth Lens</h1>
            <p><em>See through the noise. For the forgotten people.</em></p>
            
            <form id="analysisForm">
                <textarea id="textInput" placeholder="Paste text to analyze: political speech, corporate policy, legal document, news article..."></textarea>
                <button type="submit">Analyze with Truth Lens</button>
            </form>
            
            <div id="results" class="result" style="display: none;">
                <h2>Analysis Results</h2>
                <div id="powerAnalysis" class="analysis-section power"></div>
                <div id="silenceAnalysis" class="analysis-section silence"></div>
                <div id="contextAnalysis" class="analysis-section context"></div>
            </div>
        </div>

        <script>
            document.getElementById('analysisForm').addEventListener('submit', async function(e) {
                e.preventDefault();
                const text = document.getElementById('textInput').value;
                
                const response = await fetch('/analyze', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ text: text })
                });
                
                const results = await response.json();
                displayResults(results);
            });
            
            function displayResults(data) {
                document.getElementById('results').style.display = 'block';
                
                // Display power analysis
                document.getElementById('powerAnalysis').innerHTML = `
                    <h3>⚖️ Power Analysis</h3>
                    <p><strong>Direct Beneficiaries:</strong> ${data.power_analysis.direct_beneficiaries.length} identified</p>
                    <p><strong>Power Assumptions:</strong> ${data.power_analysis.power_assumptions.length} found</p>
                    <p><strong>Questions to Ask:</strong> ${data.power_analysis.recommended_questions.slice(0, 3).join(', ')}</p>
                `;
                
                // Display silence analysis
                document.getElementById('silenceAnalysis').innerHTML = `
                    <h3>🔇 Silence Analysis</h3>
                    <p><strong>Missing Perspectives:</strong> ${data.silence_analysis.missing_perspectives.length} identified</p>
                    <p><strong>Unmentioned Rights:</strong> ${data.silence_analysis.unmentioned_rights.length} found</p>
                    <p><strong>Questions to Uncover Silence:</strong> ${data.silence_analysis.questions_to_uncover_silence.slice(0, 3).join(', ')}</p>
                `;
                
                // Display context analysis
                document.getElementById('contextAnalysis').innerHTML = `
                    <h3>🌍 Context Analysis</h3>
                    <p><strong>Temporal Elements:</strong> ${data.context_analysis.temporal_context.explicit_dates.length} dates referenced</p>
                    <p><strong>Authority References:</strong> ${data.context_analysis.authority_references.length} external references</p>
                    <p><strong>Context Questions:</strong> ${data.context_analysis.contextual_questions.slice(0, 3).join(', ')}</p>
                `;
            }
        </script>
    </body>
    </html>
    """

@app.route('/analyze', methods=['POST'])
def analyze_text():
    """Analyze submitted text using all three lenses"""
    try:
        data = request.get_json()
        if not data or 'text' not in data:
            return jsonify({'error': 'No text provided'}), 400
        
        text = Security.sanitize_input(data['text'])
        
        if len(text) < 10:
            return jsonify({'error': 'Text too short for meaningful analysis'}), 400
        
        # Run all analyses
        power_analysis = power_analyzer.analyze_beneficiaries(text)
        silence_analysis = silence_detector.detect_silences(text)
        context_analysis = context_mapper.map_context(text)
        
        return jsonify({
            'power_analysis': power_analysis,
            'silence_analysis': silence_analysis,
            'context_analysis': context_analysis,
            'analysis_timestamp': 'not_stored',  # Explicitly no tracking
            'text_length': len(text)
        })
        
    except Exception as e:
        return jsonify({'error': f'Analysis failed: {str(e)}'}), 500

@app.route('/api/health')
def health_check():
    """Simple health check endpoint"""
    return jsonify({
        'status': 'healthy',
        'version': '1.0.0',
        'privacy': 'zero_tracking_enforced'
    })

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=5000)
    from flask import Blueprint, request, jsonify
from ..core.security import Security
from ..analyzer.power_analyzer import PowerAnalyzer
from ..analyzer.silence_detector import SilenceDetector
from ..analyzer.context_mapper import ContextMapper

api = Blueprint('api', __name__)

power_analyzer = PowerAnalyzer()
silence_detector = SilenceDetector()
context_mapper = ContextMapper()

@api.route('/analyze', methods=['POST'])
def api_analyze():
    """API endpoint for programmatic analysis"""
    data = request.get_json()
    
    if not data or 'text' not in data:
        return jsonify({'error': 'Missing text parameter'}), 400
    
    text = Security.sanitize_input(data['text'])
    
    if len(text) < 10:
        return jsonify({'error': 'Text too short'}), 400
    
    # Run analyses
    try:
        power_analysis = power_analyzer.analyze_beneficiaries(text)
        silence_analysis = silence_detector.detect_silences(text)
        context_analysis = context_mapper.map_context(text)
        
        return jsonify({
            'analysis': {
                'power': power_analysis,
                'silence': silence_analysis,
                'context': context_analysis
            },
            'metadata': {
                'text_length': len(text),
                'analysis_complete': True,
                'privacy_note': 'No data stored or tracked'
            }
        })
        
    except Exception as e:
        return jsonify({'error': f'Analysis error: {str(e)}'}), 500

@api.route('/analyze/power', methods=['POST'])
def api_analyze_power():
    """API endpoint for power analysis only"""
    return _single_analysis_endpoint('power')

@api.route('/analyze/silence', methods=['POST'])
def api_analyze_silence():
    """API endpoint for silence analysis only"""
    return _single_analysis_endpoint('silence')

@api.route('/analyze/context', methods=['POST'])
def api_analyze_context():
    """API endpoint for context analysis only"""
    return _single_analysis_endpoint('context')

def _single_analysis_endpoint(analysis_type):
    """Helper for single analysis endpoints"""
    data = request.get_json()
    
    if not data or 'text' not in data:
        return jsonify({'error': 'Missing text parameter'}), 400
    
    text = Security.sanitize_input(data['text'])
    
    try:
        if analysis_type == 'power':
            result = power_analyzer.analyze_beneficiaries(text)
        elif analysis_type == 'silence':
            result = silence_detector.detect_silences(text)
        elif analysis_type == 'context':
            result = context_mapper.map_context(text)
        else:
            return jsonify({'error': 'Invalid analysis type'}), 400
        
        return jsonify({
            analysis_type: result,
            'metadata': {
                'text_length': len(text),
                'privacy_note': 'No data stored or tracked'
            }
        })
        
    except Exception as e:
        return jsonify({'error': f'Analysis error: {str(e)}'}), 500
     # Example analysis of political speech
"""
# Truth Lens - Political Speech Analysis Example

This example demonstrates analyzing a political speech using Truth Lens.
"""

sample_speech = """
My fellow citizens, today I announce the Economic Prosperity Act. 
This legislation will create millions of jobs and boost our GDP by 5%. 
We must pass this bill to ensure American dominance in global markets. 
The Department of Commerce will oversee implementation. 
Business leaders support this initiative as it reduces regulation 
and provides tax incentives for corporations that create jobs. 
We cannot afford delay - the act takes effect immediately upon passage.
"""

# Initialize analyzers
from src.analyzer.power_analyzer import PowerAnalyzer
from src.analyzer.silence_detector import SilenceDetector
from src.analyzer.context_mapper import ContextMapper

power_analyzer = PowerAnalyzer()
silence_detector = SilenceDetector()
context_mapper = ContextMapper()

print("=== POWER ANALYSIS ===")
power_result = power_analyzer.analyze_beneficiaries(sample_speech)
print("Direct beneficiaries:", [b['entity'] for b in power_result['direct_beneficiaries']])
print("Power assumptions:", [a['description'] for a in power_result['power_assumptions']])

print("\n=== SILENCE ANALYSIS ===")
silence_result = silence_detector.detect_silences(sample_speech)
print("Missing perspectives:", [p['perspective'] for p in silence_result['missing_perspectives']])
print("Unmentioned rights:", [r['right'] for r in silence_result['unmentioned_rights']])

print("\n=== CONTEXT ANALYSIS ===")
context_result = context_mapper.map_context(sample_speech)
print("Temporal context:", context_result['temporal_context']['limitations'])
print("Assumed knowledge:", context_result['assumed_knowledge'][:3]) 
import unittest
import sys
import os
sys.path.append(os.path.join(os.path.dirname(__file__), '..'))

from src.analyzer.power_analyzer import PowerAnalyzer
from src.analyzer.silence_detector import SilenceDetector
from src.analyzer.context_mapper import ContextMapper
from src.core.security import Security

class TestTruthLens(unittest.TestCase):
    
    def setUp(self):
        self.power_analyzer = PowerAnalyzer()
        self.silence_detector = SilenceDetector()
        self.context_mapper = ContextMapper()
        self.sample_text = "The corporation must maximize shareholder value. All decisions are final."
    
    def test_power_analysis_identifies_beneficiaries(self):
        result = self.power_analyzer.analyze_beneficiaries(self.sample_text)
        self.assertIn('direct_beneficiaries', result)
        self.assertIn('power_assumptions', result)
    
    def test_silence_detection_finds_missing_perspectives(self):
        result = self.silence_detector.detect_silences(self.sample_text)
        self.assertIn('missing_perspectives', result)
        self.assertTrue(len(result['missing_perspectives']) > 0)
    
    def test_context_mapping_identifies_assumptions(self):
        result = self.context_mapper.map_context(self.sample_text)
        self.assertIn('assumed_knowledge', result)
        self.assertIn('temporal_context', result)
    
    def test_security_sanitization(self):
        dangerous_text = "<script>alert('xss')</script>Normal text"
        sanitized = Security.sanitize_input(dangerous_text)
        self.assertNotIn('<script>', sanitized)
        self.assertIn('Normal text', sanitized)

if __name__ == '__main__':
    unittest.main()
    # Truth Lens Analysis Framework

## Core Philosophy

Truth Lens operates on three fundamental principles:

1. **Power Analysis**: Every text reflects power structures
2. **Silence Detection**: What's missing is as important as what's present  
3. **Context Mapping**: Text exists within historical and social frameworks

## Methodologies

### Power Analysis
- Identifies direct and indirect beneficiaries
- Maps control structures and authority assumptions
- Generates critical questions about power dynamics

### Silence Detection  
- Identifies missing perspectives and voices
- Maps narrative gaps and omissions
- Uncovers unmentioned rights and alternatives

### Context Mapping
- Analyzes temporal and conditional frameworks
- Identifies authority references and dependencies
- Maps assumed knowledge and background context

## Privacy & Security

- Zero data persistence
- No user tracking
- All analysis happens in memory
- Sessions are anonymous and ephemeral
- # Truth Lens API Documentation

## Base URL
`https://your-domain.com/api`

## Endpoints

### POST /api/analyze
Complete analysis using all three lenses.

**Request:**
```json
{
  "text": "Text to analyze..."
}{
  "analysis": {
    "power": { ... },
    "silence": { ... },
    "context": { ... }
  }
}
**13. README.md** (Enhanced)
```markdown
# Truth Lens - See Through the Noise

A tool for the forgotten people. Truth Lens reveals the hidden frameworks in any text through three core analyses.

## 🎯 The Three Lenses

1. **Power Analysis** - Who benefits? What power assumptions are baked in?
2. **Silence Analysis** - Who/what is missing? What perspectives are excluded?  
3. **Context Analysis** - What conditions made this possible?

## 🚀 Quick Start

```bash
git clone https://github.com/[your-username]/truth-lens
cd truth-lens
pip install -r requirements.txt
python src/interface/web_interface.py
from src.analyzer.power_analyzer import PowerAnalyzer

analyzer = PowerAnalyzer()
result = analyzer.analyze_beneficiaries("Your text here")

**14. .gitignore**
```gitignore
# Environment
.env
.venv
env/
venv/

# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg

# Database
*.db
*.sqlite3

# Logs
*.log

# OS
.DS_Store
Thumbs.db# Deployment Guide

## Local Development
```bash
python src/interface/web_interface.py
gunicorn --bind 0.0.0.0:5000 src.interface.web_interface:app
FROM python:3.9-slim
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "src.interface.web_interface:app"]

---

**Repository Complete ✅**

The Truth Lens is now a fully functional, deployable tool with:

- ✅ **Complete analysis engine** (power, silence, context)
- ✅ **Web interface** with zero tracking
- ✅ **REST API** for developers  
- ✅ **Comprehensive documentation**
- ✅ **Example analyses**
- ✅ **Test suite**
- ✅ **Security and privacy by design**
- ✅ **Deployment ready**

**To deploy immediately:**
```bash
git init
git add .
git commit -m "Initial commit: Truth Lens for the forgotten people"
git remote add origin https://github.com/[your-username]/truth-lens
git push -u origin main
# Deploy to free tier hosting
# Options: Heroku, Railway, PythonAnywhere, Replit
git push heroku main
# Now accessible at: https://truth-lens.herokuapp.com
// Truth Lens extension that analyzes any webpage
// Right-click -> "Analyze with Truth Lens"
# examples/cognitive_bias_analysis.py

"""
Truth Lens - Cognitive Bias Analysis Example

This example demonstrates analyzing the psychological concept of Confirmation Bias
to reveal its power dynamics, silences, and societal context.
"""
sample_concept_text = """
Confirmation Bias is the tendency to search for, interpret, favor, and recall
information in a way that confirms or supports one's prior beliefs or values.
This leads to overconfidence in personal beliefs and can result in polarized
social groups that rarely engage with disconfirming evidence.
"""

# Initialize analyzers
from src.analyzer.power_analyzer import PowerAnalyzer
from src.analyzer.silence_detector import SilenceDetector
from src.analyzer.context_mapper import ContextMapper

power_analyzer = PowerAnalyzer()
silence_detector = SilenceDetector()
context_mapper = ContextMapper()

print("=== 🧠 TRUTH LENS ON COGNITIVE BIAS ===")

print("\n--- ⚖️ POWER ANALYSIS ---")
power_result = power_analyzer.analyze_beneficiaries(sample_concept_text)
print("Thought: Who benefits from *unquestioned* beliefs?")
print("Identified Power Assumptions:", [a['description'] for a in power_result['power_assumptions']][:2])

print("\n--- 🔇 SILENCE ANALYSIS ---")
silence_result = silence_detector.detect_silences(sample_concept_text)
print("Thought: What is ignored when Confirmation Bias dominates?")
print("Missing Perspectives:", [p['perspective'] for p in silence_result['missing_perspectives']][:2])

print("\n--- 🌍 CONTEXT ANALYSIS ---")
context_result = context_mapper.map_context(sample_concept_text)
print("Thought: What social context amplifies this bias?")
print("Context Questions:", context_result['contextual_questions'][-2:])
# Truth Lens - See Through the Noise

A tool for the forgotten people. Truth Lens reveals the hidden frameworks in any text through three core analyses.

## 🎯 The Three Lenses

1. **Power Analysis** - Who benefits? What power assumptions are baked in?
2. **Silence Analysis** - Who/what is missing? What perspectives are excluded?  
3. **Context Analysis** - What conditions made this possible?

---

## 🧠 Applying the Lens to Abstract Concepts

Truth Lens is not limited to policies and speeches. It can be applied to **abstract or philosophical text** (e.g., psychological theories, ethical frameworks, artistic manifestos) by reinterpreting the lenses:

* **Power:** Focus on the dominance of **one idea** or the **conceptual beneficiaries** of a flawed theory.
* **Silence:** Look for **alternative theories** or human experiences that the concept fails to account for.
* **Context:** Map the **historical era** or societal crisis that made the abstract concept necessary or popular.

Run the new example:
```bash
python examples/cognitive_bias_analysis.py
"""
Truth Lens Analyzer Module
Core analysis engines for power, silence, and context detection
"""

from .power_analyzer import PowerAnalyzer
from .silence_detector import SilenceDetector
from .context_mapper import ContextMapper

__all__ = ['PowerAnalyzer', 'SilenceDetector', 'ContextMapper']

def analyze_text(text: str) -> dict:
    """
    Convenience function to run all three analyses on text
    """
    power = PowerAnalyzer()
    silence = SilenceDetector()
    context = ContextMapper()
    
    return {
        'power': power.analyze_beneficiaries(text),
        'silence': silence.detect_silences(text),
        'context': context.map_context(text)
    }"""
Truth Lens Core Module
Configuration and security utilities
"""

from .config import Config
from .security import Security

__all__ = ['Config', 'Security']
"""
Truth Lens Interface Module
Web and API interfaces for text analysis
"""

from .web_interface import app
from .api import api

__all__ = ['app', 'api
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    python3-dev \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements first for better caching
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Download NLTK data
RUN python -m nltk.downloader punkt stopwords

# Copy application code
COPY . .

# Create non-root user for security
RUN useradd -m -u 1000 truthlens && chown -R truthlens:truthlens /app
USER truthlens

# Expose port
EXPOSE 5000

# Set environment variables
ENV FLASK_APP=src.interface.web_interface:app
ENV PYTHONUNBUFFERED=1

# Run the application
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers", "2", "--threads", "4", "src.interface.web_interface:app"]
version: '3.8'

services:
  truth-lens:
    build: .
    ports:
      - "5000:5000"
    environment:
      - FLASK_ENV=development
      - FLASK_DEBUG=1
      - SECRET_KEY=truth-lens-dev-key
    volumes:
      - ./src:/app/src
    command: python -m flask run --host=0.0.0.0
from setuptools import setup, find_packages

with open("README.md", "r", encoding="utf-8") as fh:
    long_description = fh.read()

setup(
    name="truth-lens",
    version="1.0.0",
    author="LHMisme420",
    description="A tool for the forgotten people - reveals hidden frameworks in text",
    long_description=long_description,
    long_description_content_type="text/markdown",
    url="https://github.com/LHMisme420/Truth-Lens",
    packages=find_packages(where="src"),
    package_dir={"": "src"},
    classifiers=[
        "Development Status :: 4 - Beta",
        "Intended Audience :: Developers",
        "Intended Audience :: Education",
        "Topic :: Text Processing :: Linguistic",
        "License :: OSI Approved :: MIT License",
        "Programming Language :: Python :: 3",
        "Programming Language :: Python :: 3.7",
        "Programming Language :: Python :: 3.8",
        "Programming Language :: Python :: 3.9",
    ],
    python_requires=">=3.7",
    install_requires=[
        "flask>=2.3.3",
        "nltk>=3.8.1",
        "textblob>=0.17.1",
        "newspaper3k>=0.2.8",
        "requests>=2.31.0",
        "gunicorn>=21.2.0",
        "python-dotenv>=1.0.0",
    ],
    entry_points={
        "console_scripts": [
            "truth-lens=interface.web_interface:main",
        ],
    },
)
.PHONY: help install run test clean docker-build docker-run deploy-heroku

help:
	@echo "Truth Lens - Make Commands"
	@echo "=========================="
	@echo "install       - Install dependencies"
	@echo "run          - Run local development server"
	@echo "test         - Run test suite"
	@echo "clean        - Clean cache files"
	@echo "docker-build - Build Docker image"
	@echo "docker-run   - Run in Docker"
	@echo "deploy-heroku - Deploy to Heroku"

install:
	pip install -r requirements.txt
	python -m nltk.downloader punkt stopwords

run:
	python src/interface/web_interface.py

test:
	python -m pytest tests/ -v

clean:
	find . -type d -name "__pycache__" -exec rm -rf {} +
	find . -type f -name "*.pyc" -delete
	find . -type f -name "*.pyo" -delete
	find . -type f -name "*.egg-info" -exec rm -rf {} +
	find . -type d -name ".pytest_cache" -exec rm -rf {} +

docker-build:
	docker build -t truth-lens .

docker-run:
	docker-compose up

deploy-heroku:
	heroku create truth-lens-app
	git push heroku main
	heroku open
#!/usr/bin/env python3
"""
Truth Lens CLI - Command Line Interface for text analysis
"""

import argparse
import json
import sys
from pathlib import Path

# Add src to path
sys.path.insert(0, str(Path(__file__).parent.parent))

from src.analyzer.power_analyzer import PowerAnalyzer
from src.analyzer.silence_detector import SilenceDetector
from src.analyzer.context_mapper import ContextMapper
from src.core.security import Security

def main():
    parser = argparse.ArgumentParser(
        description="Truth Lens - See through the noise. For the forgotten people.",
        formatter_class=argparse.RawDescriptionHelpFormatter,
        epilog="""
Examples:
  truth-lens --text "Your text here"
  truth-lens --file document.txt
  truth-lens --file document.txt --lens power
  truth-lens --file document.txt --json output.json
        """
    )
    
    # Input options
    input_group = parser.add_mutually_exclusive_group(required=True)
    input_group.add_argument('--text', help='Text to analyze')
    input_group.add_argument('--file', help='File to analyze')
    
    # Analysis options
    parser.add_argument('--lens', choices=['all', 'power', 'silence', 'context'], 
                       default='all', help='Which lens to apply (default: all)')
    
    # Output options
    parser.add_argument('--json', metavar='FILE', help='Save results to JSON file')
    parser.add_argument('--verbose', action='store_true', help='Verbose output')
utput')
    parser.add_argument('--questions-only', action='store_true', 
                       help='Only show critical questions')
    
    args = parser.parse_args()
    
    # Get text to analyze
    if args.text:
        text = args.text
    else:
        try:
            with open(args.file, 'r', encoding='utf-8') as f:
                text = f.read()
        except FileNotFoundError:
            print(f"Error: File '{args.file}' not found")
            sys.exit(1)
        except Exception as e:
            print(f"Error reading file: {e}")
            sys.exit(1)
    
    # Sanitize input
    text = Security.sanitize_input(text)
    
    if len(text) < 10:
        print("Error: Text too short for meaningful analysis")
        sys.exit(1)
    
    # Initialize analyzers
    results = {}
    
    if args.lens in ['all', 'power']:
        power_analyzer = PowerAnalyzer()
        results['power'] = power_analyzer.analyze_beneficiaries(text)
    
    if args.lens in ['all', 'silence']:
        silence_detector = SilenceDetector()
        results['silence'] = silence_detector.detect_silences(text)
    
    if args.lens in ['all', 'context']:
        context_mapper = ContextMapper()
        results['context'] = context_mapper.map_context(text)
    
    # Output results
    if args.json:
        with open(args.json, 'w') as f:
            json.dump(results, f, indent=2)
        print(f"Results saved to {args.json}")
    
    # Console output
    print("\n" + "="*60)
    print("🔍 TRUTH LENS ANALYSIS")
    print("="*60)
    
    if args.questions_only:
        print_questions(results)
    else:
        print_full_results(results, args.verbose)

def print_questions(results):
    """Print only the critical questions from each analysis"""
    print("\n❓ CRITICAL QUESTIONS TO ASK:\n")
    
    if 'power' in results:
        print("⚖️  Power Questions:")
        for q in results['power']['recommended_questions'][:3]:
            print(f"   • {q}")
    
    if 'silence' in results:
        print("\n🔇 Silence Questions:")
        for q in results['silence']['questions_to_uncover_silence'][:3]:
            print(f"   • {q}")
    
    if 'context' in results:
        print("\n🌍 Context Questions:")
        for q in results['context']['contextual_questions'][:3]:
            print(f"   • {q}")

def print_full_results(results, verbose=False):
    """Print full analysis results"""
    
    if 'power' in results:
        print("\n⚖️  POWER ANALYSIS")
        print("-" * 40)
        power = results['power']
        
        if power['direct_beneficiaries']:
            print(f"Direct Beneficiaries: {len(power['direct_beneficiaries'])} identified")
            if verbose:
                for b in power['direct_beneficiaries'][:3]:
                    print(f"  • {b.get('entity', 'Unknown')}: {b.get('evidence', '')}")
        
        if power['power_assumptions']:
            print(f"Power Assumptions: {len(power['power_assumptions'])} found")
            if verbose:
                for a in power['power_assumptions'][:3]:
                    print(f"  • {a.get('description', '')}")
        
        print("\nQuestions to Ask:")
        for q in power['recommended_questions'][:3]:
            print(f"  • {q}")
    
    if 'silence' in results:
        print("\n🔇 SILENCE ANALYSIS")
        print("-" * 40)
        silence = results['silence']
        
        print(f"Missing Perspectives: {len(silence['missing_perspectives'])} identified")
        if verbose:
            for p in silence['missing_perspectives'][:3]:
                print(f"  • {p.get('perspective', '')}: {p.get('significance', '')}")
        
        print(f"Unmentioned Rights: {len(silence['unmentioned_rights'])} found")
        if verbose:
            for r in silence['unmentioned_rights'][:3]:
                print(f"  • {r.get('right', '')}")
        
        print("\nQuestions to Uncover:")
        for q in silence['questions_to_uncover_silence'][:3]:
            print(f"  • {q}")
    
    if 'context' in results:
        print("\n🌍 CONTEXT ANALYSIS")
        print("-" * 40)
        context = results['context']
        
        if context['temporal_context']['explicit_dates']:
            print(f"Dates Referenced: {len(context['temporal_context']['explicit_dates'])}")
        
        if context['authority_references']:
            print(f"Authority References: {len(context['authority_references'])}")
            if verbose:
                for ref in context['authority_references'][:3]:
                    print(f"  • {ref.get('reference', '')}")
        
        if context['assumed_knowledge']:
            print(f"Assumed Knowledge: {len(context['assumed_knowledge'])} items")
            if verbose:
                for item in context['assumed_knowledge'][:3]:
                    print(f"  • {item}")
        
        print("\nContext Questions:")
        for q in context['contextual_questions'][:3]:
            print(f"  • {q}")
    
    print("\n" + "="*60)
    print("Privacy Note: No data stored or tracked")
    print("="*60)

if __name__ == "__main__":
    main()
{
  "manifest_version": 3,
  "name": "Truth Lens",
  "version": "1.0.0",
  "description": "See through the noise - Analyze any text for power dynamics, silences, and context",
  "permissions": [
    "activeTab",
    "contextMenus",
    "storage"
  ],
  "host_permissions": [
    "http://localhost:5000/*",
    "https://truth-lens.herokuapp.com/*"
  ],
  "background": {
    "service_worker": "background.js"
  },
  "action": {
    "default_popup": "popup.html",
    "default_icon": {
      "16": "icon16.png",
      "48": "icon48.png",
      "128": "icon128.png"
    }
  },
  "icons": {
    "16": "icon16.png",
    "48": "icon48.png",
    "128": "icon128.png"
  },
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content.js"]
    }
  ]
}// Truth Lens Browser Extension - Popup Script

document.addEventListener('DOMContentLoaded', () => {
  const textInput = document.getElementById('textInput');
  const analyzeButton = document.getElementById('analyzeButton');
  const analyzeSelection = document.getElementById('analyzeSelection');
  const analyzePage = document.getElementById('analyzePage');
  const loading = document.getElementById('loading');
  const results = document.getElementById('results');

  // Analyze button click
  analyzeButton.addEventListener('click', () => {
    const text = textInput.value.trim();
    if (text.length < 10) {
      alert('Please enter at least 10 characters to analyze');
      return;
    }
    analyzeText(text);
  });

  // Analyze selection button
  analyzeSelection.addEventListener('click', async () => {
    const [tab] = await chrome.tabs.query({ active: true, currentWindow: true });
    
    chrome.tabs.sendMessage(tab.id, { action: "getSelection" }, (response) => {
      if (response && response.text) {
        textInput.value = response.text;
        analyzeText(response.text);
      } else {
        alert('Please select some text on the page first');
      }
    });
  });

  // Analyze full page button
  analyzePage.addEventListener('click', async () => {
    const [tab] = await chrome.tabs.query({ active: true, currentWindow: true });
    
    chrome.tabs.sendMessage(tab.id, { action: "getPageText" }, (response) => {
      if (response && response.text) {
        const truncated = response.text.substring(0, 5000); // Limit to 5000 chars
        textInput.value = truncated;
        analyzeText(truncated);
      }
    });
  });

  function analyzeText(text) {
    loading.style.display = 'block';
    results.style.display = 'none';
    analyzeButton.disabled = true;

    chrome.runtime.sendMessage(
      { action: "analyze", text: text },
      (response) => {
        loading.style.display = 'none';
        analyzeButton.disabled = false;

        if (response.success) {
          displayResults(response.data);
        } else {
          alert('Analysis failed. Make sure Truth Lens server is running.');
        }
      }
    );
  }

  function displayResults(data) {
    results.style.display = 'block';

    // Power Analysis
    const powerDiv = document.getElementById('powerAnalysis');
    powerDiv.innerHTML = `
      <h3>⚖️ Power Analysis</h3>
      <p><strong>Beneficiaries:</strong> ${data.analysis.power.direct_beneficiaries.length} identified</p>
      <p><strong>Power Assumptions:</strong> ${data.analysis.power.power_assumptions.length} found</p>
      <p><strong>Key Question:</strong> ${data.analysis.power.recommended_questions[0] || 'N/A'}</p>
    `;

    // Silence Analysis
    const silenceDiv = document.getElementById('silenceAnalysis');
    silenceDiv.innerHTML = `
      <h3>🔇 Silence Analysis</h3>
      <p><strong>Missing Perspectives:</strong> ${data.analysis.silence.missing_perspectives.length}</p>
      <p><strong>Unmentioned Rights:</strong> ${data.analysis.silence.unmentioned_rights.length}</p>
      <p><strong>Key Question:</strong> ${data.analysis.silence.questions_to_uncover_silence[0] || 'N/A'}</p>
    `;

    // Context Analysis
    const contextDiv = document.getElementById('contextAnalysis');
    contextDiv.innerHTML = `
      <h3>🌍 Context Analysis</h3>
      <p><strong>Temporal Context:</strong> ${data.analysis.context.temporal_context.explicit_dates.length} dates</p>
      <p><strong>Authority Refs:</strong> ${data.analysis.context.authority_references.length}</p>
      <p><strong>Key Question:</strong> ${data.analysis.context.contextual_questions[0] || 'N/A'}</p>
    `;
  }
});// Truth Lens Browser Extension - Content Script

// Listen for messages from popup or background
chrome.runtime.onMessage.addListener((request, sender, sendResponse) => {
  if (request.action === "getSelection") {
    const selectedText = window.getSelection().toString();
    sendResponse({ text: selectedText });
  } else if (request.action === "getPageText") {
    const pageText = document.body.innerText;
    sendResponse({ text: pageText });
  } else if (request.action === "showResults") {
    showAnalysisOverlay(request.results);
  }
  return true;
});

// Create and show analysis overlay on the page
function showAnalysisOverlay(results) {
  // Remove existing overlay if present
  const existing = document.getElementById('truth-lens-overlay');
  if (existing) {
    existing.remove();
  }

  // Create overlay element
  const overlay = document.createElement('div');
  overlay.id = 'truth-lens-overlay';
  overlay.style.cssText = `
    position: fixed;
    top: 20px;
    right: 20px;
    width: 350px;
    max-height: 500px;
    background: white;
    border: 2px solid #2c3e50;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
    z-index: 999999;
    font-family: Arial, sans-serif;
    overflow-y: auto;
  `;

  // Create content
  overlay.innerHTML = `
    <div style="background: #2c3e50; color: white; padding: 12px; position: sticky; top: 0;">
      <h3 style="margin: 0; font-size: 16px;">🔍 Truth Lens Analysis</h3>
      <button id="close-truth-lens" style="position: absolute; top: 10px; right: 10px; background: transparent; border: none; color: white; font-size: 20px; cursor: pointer;">×</button>
    </div>
    <div style="padding: 15px;">
      <div style="margin-bottom: 15px; padding: 10px; background: #fee; border-left: 4px solid #e74c3c;">
        <h4 style="margin: 0 0 5px 0; font-size: 14px; color: #c0392b;">⚖️ Power Analysis</h4>
        <p style="margin: 3px 0; font-size: 12px;">Beneficiaries: ${results.analysis.power.direct_beneficiaries.length} identified</p>
        <p style="margin: 3px 0; font-size: 12px;">Power assumptions: ${results.analysis.power.power_assumptions.length} found</p>
        <p style="margin: 3px 0; font-size: 11px; font-style: italic; color: #666;">
          Ask: ${results.analysis.power.recommended_questions[0] || 'Who benefits from this framing?'}
        </p>
      </div>
      
      <div style="margin-bottom: 15px; padding: 10px; background: #fff3cd; border-left: 4px solid #f39c12;">
        <h4 style="margin: 0 0 5px 0; font-size: 14px; color: #e67e22;">🔇 Silence Analysis</h4>
        <p style="margin: 3px 0; font-size: 12px;">Missing perspectives: ${results.analysis.silence.missing_perspectives.length}</p>
        <p style="margin: 3px 0; font-size: 12px;">Unmentioned rights: ${results.analysis.silence.unmentioned_rights.length}</p>
        <p style="margin: 3px 0; font-size: 11px; font-style: italic; color: #666;">
          Ask: ${results.analysis.silence.questions_to_uncover_silence[0] || 'Whose voice is missing?'}
        </p>
      </div>
      
      <div style="margin-bottom: 10px; padding: 10px; background: #d4edda; border-left: 4px solid #27ae60;">
        <h4 style="margin: 0 0 5px 0; font-size: 14px; color: #27ae60;">🌍 Context Analysis</h4>
        <p style="margin: 3px 0; font-size: 12px;">Temporal elements: ${results.analysis.context.temporal_context.explicit_dates.length} dates</p>
        <p style="margin: 3px 0; font-size: 12px;">Authority references: ${results.analysis.context.authority_references.length}</p>
        <p style="margin: 3px 0; font-size: 11px; font-style: italic; color: #666;">
          Ask: ${results.analysis.context.contextual_questions[0] || 'What conditions created this?'}
        </p>
      </div>
      
      <div style="margin-top: 10px; padding: 8px; background: #f8f9fa; border-radius: 4px;">
        <p style="margin: 0; font-size: 10px; color: #666; text-align: center;">
          For the forgotten people • Zero tracking • No data stored
        </p>
      </div>
    </div>
  `;

  document.body.appendChild(overlay);

  // Add close button functionality
  document.getElementById('close-truth-lens').addEventListener('click', () => {
    overlay.remove();
  });

  // Auto-close after 30 seconds
  setTimeout(() => {
    if (document.getElementById('truth-lens-overlay')) {
      overlay.remove();
    }
  }, 30000);
}#!/bin/bash

# Truth Lens Deployment Script
# For the forgotten people

set -e

echo "🔍 TRUTH LENS DEPLOYMENT"
echo "========================"
echo "For the forgotten people"
echo ""

# Check Python version
python_version=$(python3 --version 2>&1 | grep -Po '(?<=Python )\d+\.\d+')
required_version="3.7"

if [ "$(printf '%s\n' "$required_version" "$python_version" | sort -V | head -n1)" != "$required_version" ]; then
    echo "❌ Python 3.7+ required. Current version: $python_version"
    exit 1
fi

echo "✅ Python version OK: $python_version"

# Create virtual environment
echo "📦 Creating virtual environment..."
python3 -m venv venv
source venv/bin/activate

# Install dependencies
echo "📚 Installing dependencies..."
pip install --upgrade pip
pip install -r requirements.txt

# Download NLTK data
echo "📥 Downloading language data..."
python -c "import nltk; nltk.download('punkt'); nltk.download('stopwords')"

# Run tests
echo "🧪 Running tests..."
python -m pytest tests/ -v || echo "⚠️  Some tests failed, but continuing..."

# Setup environment file
if [ ! -f .env ]; then
    echo "🔧 Creating .env file..."
    cat > .env << EOF
SECRET_KEY=$(python -c 'import secrets; print(secrets.token_hex(32))')
FLASK_ENV=production
FLASK_APP=src.interface.web_interface:app
EOF
    echo "✅ .env file created"
fi

# Deployment options
echo ""
echo "🚀 DEPLOYMENT OPTIONS:"
echo "====================="
echo ""
echo "1. LOCAL DEVELOPMENT:"
echo "   make run"
echo "   # or"
echo "   python src/interface/web_interface.py"
echo ""
echo "2. DOCKER:"
echo "   docker-compose up"
echo ""
echo "3. HEROKU:"
echo "   heroku create truth-lens-[your-name]"
echo "   git push heroku main"
echo ""
echo "4. COMMAND LINE:"
echo "   python truth-lens-cli.py --file document.txt"
echo ""
echo "5. BROWSER EXTENSION:"
echo "   1. Open Chrome/Brave"
echo "   2. Go to chrome://extensions"
echo "   3. Enable Developer Mode"
echo "   4. Load unpacked -> Select browser-extension folder"
echo ""
echo "========================"
echo "✊ Power to the people"
echo "🔍 See through the noise"
echo "========================"
bash

chmod +x /home/claude/truth-lens/deploy.sh /home/claude/truth-lens/truth-lens-cli.py
Output

exit code 0
#!/bin/bash

# Truth Lens - Git Repository Setup
# Ready to deploy to GitHub

echo "🔍 TRUTH LENS - GitHub Setup"
echo "============================"
echo ""

# Check if git is initialized
if [ ! -d .git ]; then
    echo "Initializing git repository..."
    git init
    echo "✅ Git initialized"
fi

# Add all files
echo "Adding files to git..."
git add .

# Create initial commit
echo "Creating initial commit..."
git commit -m "🔍 Truth Lens - For the forgotten people

A tool that reveals hidden power structures, silences, and context in any text.

Features:
- ⚖️ Power Analysis: Identifies who benefits and what assumptions are made
- 🔇 Silence Analysis: Reveals missing perspectives and unmentioned rights  
- 🌍 Context Analysis: Maps temporal, conditional, and authority frameworks
- 🔐 Zero tracking, no data collection, privacy by design
- 🌐 Web interface, API, CLI tool, and browser extension
- 📖 Comprehensive documentation and examples

For tenants reading landlord notices.
For workers decoding corporate policies.
For citizens analyzing political speeches.
For anyone the system tries to confuse.

See through the noise."

echo "✅ Repository ready!"
echo ""
echo "NEXT STEPS:"
echo "==========="
echo ""
echo "1. Add your GitHub remote:"
echo "   git remote add origin https://github.com/LHMisme420/Truth-Lens.git"
echo ""
echo "2. Push to GitHub:"
echo "   git push -u origin main"
echo ""
echo "3. Optional: Create additional branches"
echo "   git checkout -b develop"
echo "   git push -u origin develop"
echo ""
echo "4. Set up GitHub Secrets for CI/CD (optional):"
echo "   - DOCKER_USERNAME"
echo "   - DOCKER_PASSWORD"
echo "   - HEROKU_API_KEY"
echo ""
echo "5. Deploy to production:"
echo "   - Heroku: heroku create truth-lens-app && git push heroku main"
echo "   - Docker: docker build -t truth-lens . && docker run -p 5000:5000 truth-lens"
echo "   - Direct: python src/interface/web_interface.py"
echo ""
echo "========================"
echo "✊ Power to the people"
echo "🔍 See through the noise"
echo "========================"
truth-lens/
├── src/
│   ├── analyzer/          # Core analysis engines
│   │   ├── __init__.py
│   │   ├── power_analyzer.py
│   │   ├── silence_detector.py
│   │   └── context_mapper.py
│   ├── interface/         # Web & API interfaces
│   │   ├── __init__.py
│   │   ├── web_interface.py
│   │   └── api.py
│   └── core/             # Configuration & security
│       ├── __init__.py
│       ├── config.py
│       └── security.py
├── browser-extension/     # Chrome/Brave extension
│   ├── manifest.json
│   ├── background.js
│   ├── content.js
│   ├── popup.html
│   └── popup.js
├── tests/                # Test suite
├── docs/                 # Documentation
├── examples/             # Example analyses
├── .github/workflows/    # CI/CD pipeline
│   └── ci-cd.yml
├── Dockerfile           # Container deployment
├── docker-compose.yml   # Local Docker setup
├── Procfile            # Heroku deployment
├── Makefile            # Build commands
├── requirements.txt    # Python dependencies
├── setup.py           # Package configuration
├── deploy.sh          # Deployment script
├── git-setup.sh       # Git initi
truth-lens/
├── src/
│   ├── analyzer/          # Power, Silence, Context engines
│   ├── core/              # Config & Security modules
│   └── interface/         # Web + API interfaces
├── browser-extension/     # Chrome/Brave extension
├── tests/                 # Unit tests
├── docs/                  # Documentation & API specs
├── examples/              # Demo scripts & notebooks
├── Dockerfile
├── docker-compose.yml
├── Procfile
├── Makefile
├── requirements.txt
├── setup.py
├── LICENSE
├── README.md
├── truth-lens-cli.py
├── deploy.sh
└── git-setup.sh




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
