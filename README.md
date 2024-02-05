# Trial-CrewAI

I'm testing Crew AI by putting together a team of AI agents to work as content writers. My goal is to find and describe 10 scary places worldwide in 2023. Here's my AI agents team:

- Master Ghost Hunter
- Senior Writer content

---

# Code Here

```python
#!/usr/bin/env python
# coding: utf-8

# Install

!pip install crewai
!pip install -U duckduckgo-search
!pip install --upgrade --quiet langchain-google-genai pillow
!pip install --upgrade --quiet wikipedia

# Import

import os
from crewai import Agent, Task, Crew, Process
from langchain_google_genai import ChatGoogleGenerativeAI
# from langchain.tools import DuckDuckGoSearchRun (for duck duck search)
from langchain.tools import WikipediaQueryRun
from langchain_community.utilities import WikipediaAPIWrapper

# API

# api = Your KEY API

llm = ChatGoogleGenerativeAI(model="gemini-pro",
                             verbose=True,
                             temperature=0.6,  # ความหลากหลายของคำตอบ
                             google_api_key=api)

# search_tool = DuckDuckGoSearchRun()
search_tool = WikipediaQueryRun(api_wrapper=WikipediaAPIWrapper())

# Agent

researcher = Agent(
    role="Master Ghost Hunter",
    goal='Find the scariest places in the world and share that experiences.',
    backstory="""The seasoned Master Ghost Hunter, born into a family with a 
    mystical heritage, has explored haunted sites worldwide, from ancient European castles to 
    abandoned North American asylums and remote Asian temples. Armed with occult knowledge and advanced equipment""",
    verbose=True,  # output เพิ่มมากขึ้น
    allow_delegation=False,  # อนุญาติให้ทำงานร่วมกับ Agent ตัวอื่น
    llm=llm,
    tools=[search_tool],
)

writer = Agent(
    role='Senior Writer content',
    goal="Expertly writes stories about haunted and frightening places.",
    backstory="""Meet the mysterious blogger who passionately writes eerie tales on her blog. 
    Born with a fascination for the macabre, she draws inspiration from haunted places and folklore, 
    captivating readers with chilling stories that leave them unnerved.""",
    verbose=True,
    allow_delegation=True,
    llm=llm
)

# Task

# Create tasks for your agents
task1 = Task(
    description="""Find and tell the 10 most haunted places from around the world that you think are the scariest in 2023. 
    The story must include the name and the place as well as the story that made the place scary in detail.""",
    agent=researcher
)

task2 = Task(
    description="""Write a scary story from the information you received to make it even scarier but not distort the truth, 
    as well as make it more interesting. The final answer has to be the 10 scariest places written to attract people to read..""",
    agent=writer
)

# Crew

# Instantiate your crew with a sequential process
crew = Crew(
    agents=[researcher, writer],
    tasks=[task1, task2],
    verbose=2,  # You can set it to 1 or 2 to different logging levels
    process=Process.sequential
)

# Get your crew to work!
result = crew.kickoff()

print(result)




```
---
# Output

Task output: **10 Eerie Places That Will Haunt Your Dreams:**

1. **Houska Castle, Czech Republic:** Descend into the abyss of Houska Castle, built upon a portal to the underworld. Unearthly whispers echo through its halls, and the very air crackles with malevolent energy. Dare you venture into the gateway of Hell?

2. **The Winchester Mystery House, San Jose, California:** Unravel the enigma of the Winchester Mystery House, a labyrinth of bizarre architecture and restless spirits. Each creaking staircase and hidden passageway conceals a tragic tale of those slain by Winchester rifles. Can you unravel the mystery before the ghosts claim you too?

3. **The Tower of London, London, England:** Walk the blood-soaked grounds of the Tower of London, where the specter of Anne Boleyn still roams. Her haunting cries reverberate through the centuries, seeking justice for her untimely demise. Will you become her next victim?

4. **Poveglia Island, Venice, Italy:** Embark on a chilling journey to Poveglia Island, a forsaken isle shrouded in darkness. As you step ashore, the whispers of tormented souls fill the air. Can you withstand the malevolent forces that lurk beneath the surface?

5. **The Aokigahara Forest, Japan:** Venture into the depths of the Aokigahara Forest, known as the "Suicide Forest." As you tread through its dense undergrowth, the spirits of the departed whisper their tragic tales. Will you succumb to their despair or find a way to escape their clutches?

6. **The Catacombs of Paris, France:** Descend into the subterranean labyrinth of the Catacombs of Paris, where millions of skeletal remains lie in eternal rest. The air hangs heavy with the scent of decay as you navigate the maze-like tunnels. Can you find your way out before the spirits of the dead claim you?

7. **The Island of the Dolls, Mexico:** Journey to the Island of the Dolls, a haunting haven where hundreds of mutilated dolls hang from the trees. Their lifeless eyes seem to follow your every move, their porcelain faces contorted in eternal agony. Will you dare to venture into this realm of darkness?

8. **The Lizzie Borden House, Fall River, Massachusetts:** Step inside the infamous Lizzie Borden House, where a gruesome double murder took place in 1892. The spirits of Lizzie and her victims are said to roam the halls, seeking vengeance for the unspeakable crime that occurred within these walls. Can you uncover the truth behind the Borden murders before it consumes you?

9. **The Waverly Hills Sanatorium, Louisville, Kentucky:** Explore the abandoned Waverly Hills Sanatorium, once a haven for tuberculosis patients and now a hotbed of paranormal activity. As you roam the dilapidated corridors, you'll hear disembodied voices, catch glimpses of shadowy figures, and feel the chilling presence of the restless souls who still linger within.

10. **The Edinburgh Castle, Scotland:** Unravel the dark secrets of Edinburgh Castle, a fortress steeped in bloodshed and intrigue. From the ghostly bagpiper who haunts the ramparts to the vengeful spirits of prisoners who perished within its walls, the castle's past comes alive after dark. Will you dare to venture into its haunted chambers and uncover the chilling tales that lie in wait?

---
# Video

for more details

<a href="https://youtu.be/xRhMbdppa_4" target="_blank">
  <img src="https://img.youtube.com/vi/xRhMbdppa_4/0.jpg" alt="Video Thumbnail" width="480" height="360">
</a>

