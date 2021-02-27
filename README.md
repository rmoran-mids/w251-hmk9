# w251-hmk9 
## R Moran
## Sprin '21 Tues 2p

Submission
Please submit the nohup.out file along with screenshots of your Tensorboard indicating training progress (Blue score, eval loss) over time. Also, answer the following (simple) questions:

How long does it take to complete the training run? (hint: this session is on distributed training, so it will take a while)

- It 35 hrs and 37 minutes to do 100K steps

Do you think your model is fully trained? How can you tell?

- Yes, I believe it finished, if we look at the nohup file (we had set the training step size to 50k instead of 300k) - we see the bottom of the file explicitly indicates that training has finished. Moreover, if we look at the tensorboard, we notice that it is no longer updating the number of steps beyond 50k in any of the graphs. This is enough proof the training has completed.
Were you overfitting?

- Both the eval_loss and training_loss were converging to the same value (approx 1.6) after the training process / eval process ended. Moreover, if we look at the graphs, we conclude that while the training error reduces from 0 to 100k steps, the eval loss also reduces proportionately.  This indicate that the eval loss is reducing while the training error is also reducing. Since the training loss was reducing as steps increased, but the eval loss didnt stay flat after a while, we conclude that overfitting was not happening in the model.

Were your GPUs fully utilized?
Did you monitor network traffic (hint: apt install nmon ) ? Was network the bottleneck?

- Through the AWS console I could se the network traffic on each VM was minimal

Take a look at the plot of the learning rate and then check the config file. Can you explan this setting?

- The learning rate has warm up od approx 8000 steps. During that timeit is linearly increasing the learning rate, see the associated in the graph. After that is starts decaying the learning rate after reaching that point.  "Warmup steps are just a few updates with low learning rate before / at the beginning of training. After this warmup, you use the regular learning rate (schedule) to train your model to convergence The idea that this helps your network to slowly adapt to the data intuitively makes sense. However, theoretically, the main reason for warmup steps is to allow adaptive optimisers (e.g. Adam, RMSProp, ...) to compute correct statistics of the gradients. Therefore, a warmup period makes little sense when training with plain SGD."  (Copied form Ref. https://datascience.stackexchange.com/questions/55991/in-the-context-of-deep-learning-what-is-training-warmup-steps)


How big was your training set (mb)? How many training lines did it contain?

- There were 2 training files - train.de (size = 678 M, lines = 4562012) and train.en (size = 607 M, lines = 4562012)
- 
What are the files that a TF checkpoint is comprised of?
How big is your resulting model checkpoint (mb)?
Remember the definition of a "step". How long did an average step take?
How does that correlate with the observed network utilization between nodes?
