# Chapter 2: ROS 2 Fundamentals

The Robot Operating System (ROS) is a flexible framework for writing robot software. It is a collection of tools, libraries, and conventions that aim to simplify the task of creating complex and robust robot behavior across a wide variety of robotic platforms. ROS 2 is the second generation of ROS, redesigned from the ground up to be more suitable for production environments, multi-robot systems, and real-time applications.

## The ROS 2 Architecture

ROS 2's architecture is built around the **DDS (Data Distribution Service)** standard, a protocol designed for real-time, reliable, and high-performance machine-to-machine communication. This is a major departure from ROS 1's custom TCP-based communication, and it brings several key advantages:

*   **Decentralized Discovery:** Nodes can discover each other automatically on the network without a central master, eliminating a single point of failure.
*   **Improved Performance:** DDS provides low-latency, high-throughput data exchange.
*   **Quality of Service (QoS):** ROS 2 exposes a rich set of QoS policies that allow you to fine-tune the reliability and priority of your data streams. You can choose, for example, whether a message should be delivered reliably (like TCP) or on a best-effort basis (like UDP).

A ROS 2 system is a distributed network of independent programs called **nodes**. Each node is responsible for a specific task, such as controlling a motor, reading a sensor, or planning a path.

## Core Communication Concepts

Nodes communicate with each other using a set of well-defined mechanisms.

### Nodes
A **node** is the fundamental unit of computation in ROS 2. Think of it as a small, single-purpose program. For example, you might have one node for controlling the wheels, another for reading laser scan data, and a third for localizing the robot.

### Topics
**Topics** are the primary mechanism for asynchronous, one-to-many communication. A node that wants to share information **publishes** messages to a topic. Any number of other nodes can **subscribe** to that topic to receive the messages. This is a decoupled communication pattern; the publisher doesn't know or care who is subscribed.

For example, a camera node would publish images to an `/camera/image_raw` topic, and a separate image processing node could subscribe to it.

**Example: A Simple Publisher Node**
This node publishes a "Hello World" message to a topic every half a second.

```python
#
#
#
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class MinimalPublisher(Node):
    def __init__(self):
        super().__init__('minimal_publisher')
        # Create a publisher on the 'topic' topic for String messages
        self.publisher_ = self.create_publisher(String, 'topic', 10)
        timer_period = 0.5  # seconds
        self.timer = self.create_timer(timer_period, self.timer_callback)
        self.i = 0

    def timer_callback(self):
        msg = String()
        msg.data = 'Hello World: %d' % self.i
        self.publisher_.publish(msg)
        self.get_logger().info('Publishing: "%s"' % msg.data)
        self.i += 1

def main(args=None):
    rclpy.init(args=args)
    minimal_publisher = MinimalPublisher()
    rclpy.spin(minimal_publisher) # Keep the node alive
    minimal_publisher.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

**Example: A Simple Subscriber Node**
This node subscribes to the `topic` topic and prints the messages it receives.

```python
#
#
#
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class MinimalSubscriber(Node):
    def __init__(self):
        super().__init__('minimal_subscriber')
        # Create a subscription to the 'topic' topic
        self.subscription = self.create_subscription(
            String,
            'topic',
            self.listener_callback,
            10) # The 10 is the QoS history depth

    def listener_callback(self, msg):
        self.get_logger().info('I heard: "%s"' % msg.data)

def main(args=None):
    rclpy.init(args=args)
    minimal_subscriber = MinimalSubscriber()
    rclpy.spin(minimal_subscriber)
    minimal_subscriber.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

### Services
**Services** are used for synchronous, request-reply communication. A node offers a **service**, and another node can act as a **client** to call that service. The client sends a request and waits for a response from the service provider. This is a tightly coupled, two-way interaction, ideal for tasks that need to be completed before the program continues.

Example: A node might provide a service called `/set_robot_pose` that, when called, moves the robot to a specific location and returns a `success` boolean.

### Actions
**Actions** are for long-running, asynchronous tasks that provide feedback. They are like services, but with the ability to be preempted (cancelled) and to provide a stream of feedback to the client while they are executing.

Example: Navigating to a goal is a perfect use case for an action. The client sends a goal (e.g., "go to the kitchen"). The action server starts navigating, providing feedback along the way (e.g., "distance to goal: 5 meters"). The client can cancel the goal at any time. When the robot arrives, the server returns a final result.

## Building ROS 2 Packages
ROS 2 code is organized into **packages**. A package is a directory containing your nodes, configuration files, and a `package.xml` file that describes the package and its dependencies.

To build your code, ROS 2 uses a build system like `colcon`. You organize your packages into a **workspace**, and `colcon` will build them all, handling dependencies between them and generating the necessary setup files to make your executables and libraries available to the ROS 2 environment. A typical Python package would have a structure like this:
```
my_python_pkg/
├── package.xml
├── setup.py
├── setup.cfg
└── my_python_pkg/
    ├── __init__.py
    ├── publisher_node.py
    └── subscriber_node.py
```
## Launch Files
A complex robotic system can consist of dozens of nodes, each with its own set of configurations. Starting them all manually would be tedious and error-prone. **Launch files** are the solution. They are Python scripts that allow you to describe and run a complex system of nodes.

With a launch file, you can:
-   Start multiple nodes at once.
-   Automatically set parameters for each node.
-   Remap topic names.
-   Group nodes into namespaces for better organization.

This powerful system is essential for managing the complexity of real-world robotic applications.