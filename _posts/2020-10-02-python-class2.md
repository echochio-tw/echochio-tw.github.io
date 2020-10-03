---
layout: post
title: python class 雜亂記
date: 2020-10-02
tags: python class
---

```
class Animal:
    name = ""
    category = ""
    
    def __init__(self, name):
        self.name = name
    
    def set_category(self, category):
        self.category = category
class Turtle(Animal):
    category = "reptile"
class Snake(Animal):
    category = "reptile"

class Zoo:
    def __init__(self):
        self.current_animals = {}
    
    def add_animal(self, animal):
        self.current_animals[animal.name] = animal.category
        # print (self.current_animals)
    
    def total_of_category(self, category):
        result = 0
        # print (self.current_animals)
        for animal in self.current_animals.values():
            if animal == category:
              result += 1
        return result
        
zoo = Zoo()
turtle = Turtle("Turtle")
snake = Snake("Snake")
zoo.add_animal(turtle)
zoo.add_animal(snake)
print(zoo.total_of_category("reptile"))
```

亂寫...結果對不管了
```
import random

class Server:
  def __init__(self):
    """Creates a new server instance, with no active connections."""
    self.connections = {}

  def add_connection(self, connection_id):
    """Adds a new connection to this server."""
    self.connections[connection_id] = random.random()*10+1
    # Add the connection to the dictionary with the calculated load

  def close_connection(self, connection_id):
    """Closes a connection on this server."""
    del self.connections[connection_id]
    # Remove the connection from the dictionary

  def load(self):
    """Calculates the current load for all connections."""
    # Add up the load for each of the connections
    return len(self.connections)

  def __str__(self):
    """Returns a string with the current load of the server"""
    return "{:.2f}%".format(self.load())

class LoadBalancing:
  def __init__(self):
    """Initialize the load balancing system with one server"""
    self.connections = {}
    self.servers = [Server()]
        
  def add_connection(self, connection_id):
    """Randomly selects a server and adds a connection to it."""
    server = random.choice(self.servers)
    server.add_connection(connection_id)

  def close_connection(self, connection_id):
    """Closes the connection on the the server corresponding to connection_id."""
    for server in self.servers:
      for i in server.connections.keys():
        if ( i == connection_id):
          server.close_connection(i)
          return

  def avg_load(self):
    """Calculates the average load of all servers"""
    # Sum the load of each server and divide by the amount of servers
    sum = 0
    length = 0
    for server in self.servers:
      sum = server.load()+sum
      length = length + 1
    avg = sum / length
    return avg

  def ensure_availability(self):
    """If the average load is higher than 50, spin up a new server"""
    sum = 0
    length = 0
    for server in self.servers:
      sum = server.load()+sum
      length = length + 1
    avg = sum / length
    if avg >= 50:
      self.servers.append(Server())

  def __str__(self):
    """Returns a string with the load for each server."""
    loads = [str(server) for server in self.servers]
    return "[{}]".format(",".join(loads))


l = LoadBalancing()
l.add_connection("fdca:83d2::f20d")
print(l.avg_load())
l.servers.append(Server())
print(l.avg_load())
l.close_connection("fdca:83d2::f20d")
print(l.avg_load())
for connection in range(200):
    l.add_connection(connection)
    l.ensure_availability()
print(l)
print(l.avg_load())
```

```
def get_event_date(event):
  return event.date

def current_users(events):
  events.sort(key=get_event_date)
  machines = {}
  for event in events:
    if event.machine not in machines:
      machines[event.machine] = set()
    if event.type == "login":
      machines[event.machine].add(event.user)
    elif event.type == "logout":
      try:
        machines[event.machine].remove(event.user)
      except:
        print("warning {} not log in !! cant not logout !!".format(event.user))
  return machines

def generate_report(machines):
  for machine, users in machines.items():
    if len(users) > 0:
      user_list = ", ".join(users)
      print("{}: {}".format(machine, user_list))

class Event:
  def __init__(self, event_date, event_type, machine_name, user):
    self.date = event_date
    self.type = event_type
    self.machine = machine_name
    self.user = user

events = [
    Event('2020-01-21 12:45:56', 'login', 'myworkstation.local', 'jordan'),
    Event('2020-01-22 15:53:42', 'logout', 'webserver.local', 'jordan'),
    Event('2020-01-21 18:53:21', 'login', 'webserver.local', 'lane'),
    Event('2020-01-22 10:25:34', 'logout', 'myworkstation.local', 'jordan'),
    Event('2020-01-21 08:20:01', 'login', 'webserver.local', 'jordan'),
    Event('2020-01-23 11:24:35', 'logout', 'mailserver.local', 'chris'),
]

users = current_users(events)
print(users)
generate_report(users)
```
