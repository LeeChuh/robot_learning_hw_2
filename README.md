# robot_learning_hw_2

# 1. Collaborators

Ziyao Shangguan

Eason Ding 

Fred Yang 

# 2. Algorithms being used
We choose to use InfoGAIL. Similar to GAN-like structure, InfoGAIL tries to generate a policy (policy generator) that mimic expert's behavior, where a discriminator tries to distinguish generator's behavior and demonstrater's behavior. Also, InfoGAIL can be used to capture and reproduce multiple modes of behavior from the same set of demonstrations. Other imitation models, such as skill tree, needs to pre-define some trajectory segmentations in order for IL model to learn, which might be hard for our tasks, since we have 4 task to learn. Also, InfoGAIL is robust and it is useful for complex tasks in the field such as stack, coffee, and kitchen, where some traditional IL algorithms might fall short with. Hence, we choose to use InfoGAIL as the skeleton of this assignment. 

# 3. 3 parameters we assess

In this assignment, rather than the original InfoGAIL that uses TRPO (Trust Region Policy Optimization) for optimization, we simply use `Adam` and `AdamW` as our main optimizer. And we use 2 coefficient `lambda_1` and `lambda_2` to balance the discriminator loss and generator loss. We also fine-tune our learning rate for both generator and discriminator.

## 3.1 `lambda_1` vs `lambda_2`

- `lambda_1` is associated with the **weight** of discriminator loss where `lambda_2` is associated with the **generator** loss. Empirically, we set `lambda_1` = 0.15 and `lambda_2` = 0.5. If we set the **weight** of discriminator too high, discriminator may be perform too well in distinguishing the expert behavior and the generated behavior.

## 3.2 `learning rate`
- `learning rate`, similar reasons as above, if the learning rate of discriminator is too large, model performs too well in distinguishing them.

# 4. Hypothesis
## 4.1 D0 vs. D1
We hypothesize that the low-variance dataset (D0) is better for InfoGAIL to generate the policy. Since D0 is a low-variance version, the quality of demonstration is better and the noise is less, which is good for InfoGAIL to generate a close-to-expert behavior.

## 4.2 Amount of data to train it
Since D1 is a high-variance version of demonstration, to train a similar-quality behavior agent, we might need more data. This is similar to central limit theorem (CLT) -- if data is noise, the more data you have, the less biased estimator you will have.

# 5. Modification
We use a simpler version of the algorithm, i.e., we only use `Adam` and `AdamW` to optimize the problem rather than using `TRPO`. For the discriminator, we apply self spatial attention for distinguishing the generated and expert behavior.

# 6. Result graph
From the graph below, we can see the discrminator's loss is pretty stable, where the generator's loss is somehow decreasing, indicating that discriminator might be in the equilibrium, where generator is keep optimizing. The reason that the algorithm might fail is the model complexity of generator. Since we are using a feed-forward neural network to predict the policy. We can use a more advanced generator to learn the distribution of the latent space. 
![image](https://github.com/LeeChuh/robot_learning_hw_2/assets/60937990/eaeff012-8f5b-469b-85a0-b03ffa74f4b4)

# 7. Result analysis
- We try to add `robot0_eye_in_hand_image` into policy training and discriminator training. However, the result got worse. One hypothesis is when the robot arm got rotated, this camera might not capture the full information of the object information.
- We are surprised that the model cannot perform well in simple stack task.
- We might switch to a better model for generation, such as variational inference model.

# Videos
## Stack
### D0

https://github.com/LeeChuh/robot_learning_hw_2/assets/60937990/38dff63f-0e8f-4007-97df-1621ac796f0c








