# Medicine-Description-Bot 
# Install Rasa using: pip install rasa

# Create a new Rasa project
rasa init --no-prompt

# Replace the contents of `data/nlu.yml` with sample training data for medication-related queries

# Example content of `data/nlu.yml`

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
