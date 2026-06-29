Agents Project
This project contains a collection of simple Python agents designed to demonstrate modularity, dynamic reloading, and unit testing within a Python package structure.

Project Structure
The agents directory is set up as a Python package with the following key files:

agents/
├── __init__.py
├── another_agent.py
├── example_agent.py
├── my_agent.py
├── test_another_agent.py
├── test_my_agent.py
└── test_new_feature.py
__init__.py: Initializes the agents directory as a Python package and exposes MyAgent and AnotherAgent for direct import.
example_agent.py: Contains SimpleAgent, a basic agent for demonstration purposes.
my_agent.py: Defines the MyAgent class with various text processing functionalities.
another_agent.py: Defines the AnotherAgent class with different interaction methods.
test_*.py: Contains unit tests for the respective agent classes.
Agents Available
MyAgent
A versatile agent for processing textual prompts. It includes the following methods:

run(prompt: str) -> str: Processes a given prompt.
word_count(prompt: str) -> int: Returns the number of words in a prompt.
reverse_prompt(prompt: str) -> str: Reverses the given prompt string.
character_frequency(prompt: str) -> dict: Calculates the frequency of each character in a string.
censor_words(prompt: str, words_to_censor: list[str]) -> str: Replaces specified words in a prompt with [CENSORED].
AnotherAgent
An agent designed for different types of interactions. It includes the following methods:

run(prompt: str) -> str: Processes a given prompt, converting it to uppercase.
greet(name: str) -> str: Returns a greeting message for a given name.
farewell(name: str) -> str: Returns a farewell message for a given name.
How to Use
To use the agents, ensure the agents directory is properly set up as a Python package (i.e., contains an __init__.py file) and is accessible in your Python path. You can then import agents directly:

from agents import MyAgent, AnotherAgent

# Instantiate MyAgent
my_agent = MyAgent()
print(my_agent.run("hello world"))
print(my_agent.word_count("this is a test"))

# Instantiate AnotherAgent
another_agent = AnotherAgent()
print(another_agent.greet("User"))
print(another_agent.run("important message"))
If you modify the agent code files, you might need to reload the modules in an interactive environment like Colab or a Jupyter Notebook to reflect the changes:

import importlib
import sys

# Reload specific agent modules
if 'agents.my_agent' in sys.modules:
    importlib.reload(sys.modules['agents.my_agent'])

# Reload the parent 'agents' package
if 'agents' in sys.modules:
    importlib.reload(sys.modules['agents'])

# Re-import to get the updated class definitions
from agents import MyAgent
Testing
Unit tests are provided for MyAgent and AnotherAgent to ensure their functionalities work as expected. These tests are located in test_my_agent.py, test_another_agent.py, and test_new_feature.py.

To run all unit tests:

import unittest
import importlib.util
import sys
from pathlib import Path

agents_dir = Path("agents")
test_files = sorted(agents_dir.glob("test_*.py"))

all_tests_suite = unittest.TestSuite()

for test_file in test_files:
    module_name = f"agents.{test_file.stem}"
    spec = importlib.util.spec_from_file_location(module_name, test_file)
    test_module = importlib.util.module_from_spec(spec)
    sys.modules[spec.name] = test_module
    spec.loader.exec_module(test_module)

    for name in dir(test_module):
        obj = getattr(test_module, name)
        if isinstance(obj, type) and issubclass(obj, unittest.TestCase) and obj != unittest.TestCase:
            loader = unittest.TestLoader()
            suite = loader.loadTestsFromTestCase(obj)
            all_tests_suite.addTest(suite)

runner = unittest.TextTestRunner(verbosity=2)
runner.run(all_tests_suite)
This will discover and run all test cases defined in the test_*.py files within the agents directory, providing detailed output on the test results.
