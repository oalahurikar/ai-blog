- Aquire knowledge and learn new tasks along their entire operational life. 
- Transfer learning:    Learning from human demonstration  transferring learn skills to new tasks.
- Transferring learn skills from simulation to real robots.

Beyond visual data, robot learning needs datasets of robot actions in the form of trajectories and
interaction force profiles associated with various tasks. Datasets on specific robot bodies and tasks do exist, but they are typically too narrow for large-scale machine learning.

Discrepancy between the robot’s performance in the real world and in the simulated environment, remains a challenge.

**Lifelong learning**
A primary long-term goal is to move beyond initial training datasets and empower robots to **continuously acquire new knowledge and skills throughout their entire operational life**. This concept, known as lifelong learning, aims to approximate how living organisms learn, preparing robots for the vast complexity and variability of the real world that a finite training set could never capture

**Knowledge Management**: Critical questions arise, such as how to ensure the robot doesn't forget important old skills while learning new ones (a problem known as catastrophic forgetting) and how to decide what information can be discarded to make room for new knowledge.

**Scaling and Adaptation**: Robots will not remain static; their physical forms may change over time with new parts like different grippers or motors. A significant challenge is the lack of robust algorithms that can automatically transfer acquired knowledge to a slightly modified platform without needing complete retraining or extensive human intervention

**Transfer Learning**
Closely related to lifelong learning is the critical challenge of **transfer learning**, which is the ability to transfer knowledge from one domain to another. This is fundamental to human intelligence and is seen as a key to achieving scalability in robotics. Future robots must be able to transfer what they learn across different tasks, environments, and even different robot bodies

**Safe Exploration**: Since it's impossible to simulate every real-world scenario, robots will need to perform live exploration to learn. A major challenge is developing methods for robots to explore their environment safely and effectively, without causing harm to themselves or their surroundings, throughout their lifetime