<h1>ExpNo 1 :Developing AI Agent with PEAS Description</h1>
<h3>Name: SUDHIR KUMAR. R</h3>
<h3>Register Number : 212223230221</h3>


## AIM:
<br>
<p>To find the PEAS description for the given AI problem and develop an AI agent.</p>
<br>
<h3>Theory</h3>
<h3>Medicine prescribing agent:</h3>
<p>Such this agent prescribes medicine for fever (greater than 98.5 degrees) which we consider here as unhealthy, by the user temperature input, and another environment is rooms in the hospital (two rooms). This agent has to consider two factors one is room location and an unhealthy patient in a random room, the agent has to move from one room to another to check and treat the unhealthy person. The performance of the agent is calculated by incrementing performance and each time after treating in one room again it has to check another room so that the movement causes the agent to reduce its performance. Hence, agents prescribe medicine to unhealthy.</p>
<hr>
<h3>PEAS DESCRIPTION:</h3>
<table>
  <tr>
    <td><strong>Agent Type</strong></td>
    <td><strong>Performance</strong></td>
     <td><strong>Environment</strong></td>
    <td><strong>Actuators</strong></td>
    <td><strong>Sensors</strong></td>
  </tr>
    <tr>
    <td><strong>Medicine prescribing agent</strong></td>
    <td><strong>Treating unhealthy, agent movement</strong></td>
     <td><strong>Rooms, Patient</strong></td>
    <td><strong>Medicine, Treatment</strong></td>
    <td><strong>Location, Temperature of patient</strong></td>
  </tr>
</table>
<hr>

## DESIGN STEPS :
<h3>STEP 1:Identifying the input:</h3>
<p>Temperature from patients, Location.</p>
<h3>STEP 2:Identifying the output:</h3>
<p>Prescribe medicine if the patient in a random has a fever.</p>
<h3>STEP 3:Developing the PEAS description:</h3>
<p>PEAS description is developed by the performance, environment, actuators, and sensors in an agent.</p>
<h3>STEP 4:Implementing the AI agent:</h3>
<p>Treat unhealthy patients in each room. And check for the unhealthy patients in random room</p>
<h3>STEP 5:</h3>
<p>Measure the performance parameters: For each treatment performance incremented, for each movement performance decremented</p>

## PROGRAM :
```python
import random

class Thing:
    def is_alive(self):
        return hasattr(self, "alive") and self.alive

    def show_state(self):
        print("I don't know how to show_state.")


class Agent(Thing):
    def __init__(self, program=None):
        self.alive = True
        self.performance = 0
        self.program = program

    def can_grab(self, thing):
        return False


def TableDrivenAgentProgram(table):
    percepts = []

    def program(percept):
        percepts.append(percept)
        return table.get(tuple(percepts))
    
    return program
loc_A, loc_B = (0, 0), (1, 0)

def TableDrivenVacuumAgent():
    table = {
        ((loc_A, "Clean"),): "Right",
        ((loc_A, "Dirty"),): "Suck",
        ((loc_B, "Clean"),): "Left",
        ((loc_B, "Dirty"),): "Suck",
        ((loc_A, "Dirty"), (loc_A, "Clean")): "Right",
        ((loc_A, "Clean"), (loc_B, "Dirty")): "Suck",
        ((loc_B, "Clean"), (loc_A, "Dirty")): "Suck",
        ((loc_B, "Dirty"), (loc_B, "Clean")): "Left",
        ((loc_A, "Dirty"), (loc_A, "Clean"), (loc_B, "Dirty")): "Suck",
        ((loc_B, "Dirty"), (loc_B, "Clean"), (loc_A, "Dirty")): "Suck",
    }
    return Agent(TableDrivenAgentProgram(table))


class Environment:
    def __init__(self):
        self.things = []
        self.agents = []

    def percept(self, agent):
        raise NotImplementedError

    def execute_action(self, agent, action):
        raise NotImplementedError

    def default_location(self, thing):
        return None

    def is_done(self):
        return not any(agent.is_alive() for agent in self.agents)

    def step(self):
        if not self.is_done():
            actions = []
            for agent in self.agents:
                if agent.alive:
                    percept = self.percept(agent)
                    action = agent.program(percept)
                    print(f"Percept: {percept} -> Action: {action}")
                    actions.append(action)
                else:
                    actions.append("")
            for agent, action in zip(self.agents, actions):
                self.execute_action(agent, action)

    def run(self, steps=1000):
        for _ in range(steps):
            if self.is_done():
                return
            self.step()

    def add_thing(self, thing, location=None):
        if not isinstance(thing, Thing):
            thing = Agent(thing)
        if thing in self.things:
            print("Can't add the same thing twice")
        else:
            thing.location = location if location is not None else self.default_location(thing)
            self.things.append(thing)
            if isinstance(thing, Agent):
                thing.performance = 0
                self.agents.append(thing)

    def delete_thing(self, thing):
        try:
            self.things.remove(thing)
        except ValueError as e:
            print(e)
        if thing in self.agents:
            self.agents.remove(thing)


class TrivialVacuumEnvironment(Environment):
    def __init__(self):
        super().__init__()
        self.status = {
            loc_A: random.choice(["Clean", "Dirty"]),
            loc_B: random.choice(["Clean", "Dirty"])
        }

    def thing_classes(self):
        return [TableDrivenVacuumAgent]

    def percept(self, agent):
        return agent.location, self.status[agent.location]

    def execute_action(self, agent, action):
        if action == "Right":
            agent.location = loc_B
            agent.performance -= 1
        elif action == "Left":
            agent.location = loc_A
            agent.performance -= 1
        elif action == "Suck":
            if self.status[agent.location] == "Dirty":
                self.status[agent.location] = "Clean"
                agent.performance += 10

    def default_location(self, thing):
        return random.choice([loc_A, loc_B])


if __name__ == "__main__":
    agent = TableDrivenVacuumAgent()
    environment = TrivialVacuumEnvironment()
    environment.add_thing(agent)

    print("Initial Environment:", environment.status)
    environment.run(steps=10)
    print("Final Environment:", environment.status)
    print("Agent Performance:", agent.performance)
```
## OUTPUT :

![image](https://github.com/user-attachments/assets/b7fc776a-c821-4531-a8e3-aee21f3519df)

## RESULT :
The Program is excuted successfully.
