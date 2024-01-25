# Medicine-Description-Bot 
# Install Rasa using: pip install rasa

# Create a new action to fetch medication information
# In actions.py
```python
from typing import Any, Text, Dict, List
from rasa_sdk import Action, Tracker
from rasa_sdk.executor import CollectingDispatcher

class ActionMedicationInfo(Action):
    def name(self) -> Text:
        return "action_medication_info"

    def run(
        self, dispatcher: CollectingDispatcher, tracker: Tracker, domain: Dict[Text, Any]
    ) -> List[Dict[Text, Any]]:
        # Extract medication name from user message
        medication = next(tracker.get_latest_entity_values("medication"), None)

        if medication:
            # Fetch medication information from your database or API
            # Replace this with actual logic to retrieve medication details
            medication_info = f"Information about {medication}: Lorem ipsum..."

            # Send the medication information to the user
            dispatcher.utter_message(text=medication_info)
        else:
            dispatcher.utter_message(text="I'm sorry, I couldn't identify the medication.")

        return []
from flask import Flask, request, jsonify

app = Flask(__name__)

# Replace this with your actual database or API integration
medication_database = {
    "aspirin": {
        "usage": "Take one tablet with water.",
        "side_effects": "Common side effects include stomach upset.",
        "contraindications": "Do not use if allergic to aspirin."
    },
    "ibuprofen": {
        "usage": "Take with food.",
        "side_effects": "May cause stomach bleeding.",
        "contraindications": "Avoid if you have a history of heart problems."
    },
    "paracetamol": {
        "usage": "Take as directed on the label.",
        "side_effects": "Generally well-tolerated.",
        "contraindications": "Avoid if you have liver disease."
    }
}

@app.route('/get_medication_info', methods=['POST'])
def get_medication_info():
    data = request.get_json()
    medication_name = data.get('medication')

    if medication_name in medication_database:
        medication_info = medication_database[medication_name]
        return jsonify({"medication_info": medication_info})
    else:
        return jsonify({"error": "Medication not found"}), 404

if __name__ == '__main__':
    app.run(debug=True)

