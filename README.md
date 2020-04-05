# FinalProject
Final project for CFG

## Upload the data
import requests
from pprint import pprint

character_id = input("What is your character's ID? ")
url = 'https://swapi.co/api/people/{}/'.format(character_id)
response = requests.get(url)
print(response)
character = response.json()
pprint(character)
