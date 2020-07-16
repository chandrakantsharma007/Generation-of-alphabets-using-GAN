# Generation-of-alphabets-using-GAN

So, what we are going to do is we are generating alphabets using GAN(Generaive Adversarial Network) in Tensorfolw 2.0.
# What is GAN?

Let us take an analogy to explain the concept:

If you want to get better at something, say chess; what would you do? You would compete with an opponent better than you. Then you would analyze what you did wrong, what he / she did right, and think on what could you do to beat him / her in the next game.

You would repeat this step until you defeat the opponent. This concept can be incorporated to build better models. So simply, for getting a powerful hero (viz generator), we need a more powerful opponent (viz discriminator)!



# How do GANs work?

We got a high level overview of GANs. Now, we will go on to understand their nitty-gritty of these things.

As we saw, there are two main components of a GAN – Generator Neural Network and Discriminator Neural Network.

![alt text](https://cdn.analyticsvidhya.com/wp-content/uploads/2017/06/11000153/g1.jpg)

The Generator Network takes an random input and tries to generate a sample of data. In the above image, we can see that generator G(z) takes a input z from p(z), where  z is a sample from probability distribution p(z). It then generates a data which is then fed into a discriminator network D(x). The task of Discriminator Network is to take input either from the real data or from the generator and try to predict whether the input is real or generated. It takes an input x from pdata(x) where pdata(x) is our real data distribution. D(x) then solves a binary classification problem using sigmoid function giving output in the range 0 to 1.

Let us define the notations we will be using to formalize our GAN,

Pdata(x) -> the distribution of real data
X -> sample from pdata(x)
P(z) -> distribution of generator
Z -> sample from p(z)
G(z) -> Generator Network
D(x) -> Discriminator Network


Now the training of GAN is done (as we saw above) as a fight between generator and discriminator. This can be represented mathematically as

![alt text](https://cdn.analyticsvidhya.com/wp-content/uploads/2017/06/14180916/g22.png)

In our function V(D, G) the first term is entropy that the data from real distribution (pdata(x)) passes through the discriminator (aka best case scenario). The discriminator tries to maximize this to 1. The second term is entropy that the data from random input (p(z)) passes through the generator, which then generates a fake sample which is then passed through the discriminator to identify the fakeness (aka worst case scenario). In this term, discriminator tries to maximize it to 0 (i.e. the log probability that the data from generated is fake is equal to 0). So overall, the discriminator is trying to maximize our function V.

On the other hand, the task of generator is exactly opposite, i.e. it tries to minimize the function V so that the differentiation between real and fake data is bare minimum. This, in other words is a cat and mouse game between generator and discriminator!

Note: This method of training a GAN is taken from game theory called the minimax game.

# Parts of training GAN
So broadly a training phase has two main subparts and they are done sequentially


Pass 1: Train discriminator and freeze generator (freezing means setting training as false. The network does only forward pass and no backpropagation is applied)
![alt text](https://cdn.analyticsvidhya.com/wp-content/uploads/2017/06/14204626/s21.png)

Pass 2: Train generator and freeze discriminator
![alt text](https://cdn.analyticsvidhya.com/wp-content/uploads/2017/06/14180916/g22.png)
Steps to train a GAN
Step 1: Define the problem. Do you want to generate fake images or fake text. Here you should completely define the problem and collect data for it.

Step 2: Define architecture of GAN. Define how your GAN should look like. Should both your generator and discriminator be multi layer perceptrons, or convolutional neural networks? This step will depend on what problem you are trying to solve.

Step 3: Train Discriminator on real data for n epochs. Get the data you want to generate fake on and train the discriminator to correctly predict them as real. Here value n can be any natural number between 1 and infinity.

Step 4: Generate fake inputs for generator and train discriminator on fake data. Get generated data and let the discriminator correctly predict them as fake.

Step 5: Train generator with the output of discriminator. Now when the discriminator is trained, you can get its predictions and use it as an objective for training the generator. Train the generator to fool the discriminator.

Step 6: Repeat step 3 to step 5 for a few epochs.

Step 7: Check if the fake data manually if it seems legit. If it seems appropriate, stop training, else go to step 3. This is a bit of a manual task, as hand evaluating the data is the best way to check the fakeness. When this step is over, you can evaluate whether the GAN is performing well enough.

