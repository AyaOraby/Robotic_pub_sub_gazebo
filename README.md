# 🚀 Custom ROS Publisher & Subscriber for TurtleBot3

## 📌 Overview
This project is all about making TurtleBot3 move using a custom ROS message! Instead of sending velocity commands directly, we create a **custom message** that lets us define movement direction and speed, making the communication between nodes more structured and flexible.

We’ll build two key components:
1. **A publisher node** that takes user input (direction & speed) and sends it as a message.
2. **A subscriber node** that listens to this message and translates it into movement commands for the robot.

Everything runs in a **Gazebo simulation**, so you can see your TurtleBot3 move based on your commands! 🎮

---

## 📂 Project Structure
```
turtlebot3_custom_control/
│── msg/
│   ├── Command.msg  # Custom message definition
│── scripts/
│   ├── turtle_control_publisher.py  # Publishes movement commands
│   ├── turtle_control_subscriber.py  # Subscribes & moves TurtleBot3
│── CMakeLists.txt  # Configures message compilation
│── package.xml  # Defines dependencies
```

---

## 🔧 Custom Message Definition
To make communication more organized, we define a **custom message** named `Command.msg`:

```plaintext
string direction  # Movement direction: "forward", "backward", "left", "right"
float32 speed     # Movement speed (e.g., 0.1 to 1.0)
```

This message is published by the **publisher node** and read by the **subscriber node**, which then translates it into actual movement.

---

## 📡 How the Nodes Work

### 📝 Publisher Node (`turtle_control_publisher.py`)
This script:
1. Initializes a ROS node.
2. Asks the user for movement direction and speed.
3. Publishes this data as a `Command` message.
4. Runs in a loop, letting the user send new commands.

```python
msg = Command()
msg.direction = direction
msg.speed = speed
pub.publish(msg)
```

🔹 **Input Example:**
```plaintext
Enter direction (forward/backward/left/right): forward
Enter speed (0.1 to 1.0): 0.5
```

---

### 🎯 Subscriber Node (`turtle_control_subscriber.py`)
This script:
1. Listens for messages on the `/turtlebot3_command` topic.
2. Reads the direction and speed from the received message.
3. Converts the data into `Twist` messages for TurtleBot3’s movement.
4. Publishes these commands to `/cmd_vel` to control the robot.

```python
if msg.direction == "forward":
    velocity_msg.linear.x = msg.speed
elif msg.direction == "backward":
    velocity_msg.linear.x = -msg.speed
elif msg.direction == "left":
    velocity_msg.angular.z = msg.speed
elif msg.direction == "right":
    velocity_msg.angular.z = -msg.speed
pub.publish(velocity_msg)
```

---

## 🛠️ Running the Simulation
1. **Launch Gazebo**:
   ```bash
   export TURTLEBOT3_MODEL=burger
   roslaunch turtlebot3_gazebo turtlebot3_empty_world.launch
   ```
2. **Run the publisher node**:
   ```bash
   rosrun turtlebot3_custom_control turtle_control_publisher.py
   ```
3. **Run the subscriber node**:
   ```bash
   rosrun turtlebot3_custom_control turtle_control_subscriber.py
   ```
4. **Watch TurtleBot3 move 🚀**




