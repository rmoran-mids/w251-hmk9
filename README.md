# w251-hmk9 
## R Moran
## Sprin '21 Tues 2p

Submission
Please submit the nohup.out file along with screenshots of your Tensorboard indicating training progress (Blue score, eval loss) over time. Also, answer the following (simple) questions:

How long does it take to complete the training run? (hint: this session is on distributed training, so it will take a while)

- It 35 hrs and 37 minutes to do 100K steps

Do you think your model is fully trained? How can you tell?

- Yes, I it finished, if you tail the nohup file (set the max step size to 100k instead of 300k) - we see the bottom of the file explicitly indicates that training has finished.
 
![Nohup Tail](https://user-images.githubusercontent.com/64815523/109389194-7882b380-78d9-11eb-90ec-2d39721d366a.JPG)

Were you overfitting?

- Both the eval_loss and training_loss were converging to the same value (approx 1.6) after the training process / eval process ended. Moreover, if we look at the graphs, we conclude that while the training error reduces from 0 to 100k steps, the eval loss also reduces proportionately.  This indicate that the eval loss is reducing while the training error is also reducing. Since the training loss was reducing as steps increased, but the eval loss didnt stay flat after a while, we conclude that overfitting was not happening in the model.

![100k Eval Loss](https://user-images.githubusercontent.com/64815523/109389203-82a4b200-78d9-11eb-984f-13151b00d15b.JPG)

![100k Train Loss](https://user-images.githubusercontent.com/64815523/109389213-8fc1a100-78d9-11eb-8330-f8f651c729ed.JPG)

Were your GPUs fully utilized?

I believe they were based on the duration of the 100k steps. 

Did you monitor network traffic (hint: apt install nmon ) ? Was network the bottleneck?

- Through the AWS console I could se the network traffic on each VM was minimal

Take a look at the plot of the learning rate and then check the config file. Can you explan this setting?

- The learning rate has warm up od approx 8000 steps. During that timeit is linearly increasing the learning rate, see the associated in the graph. After that is starts decaying the learning rate after reaching that point.  "Warmup steps are just a few updates with low learning rate before / at the beginning of training. After this warmup, you use the regular learning rate (schedule) to train your model to convergence The idea that this helps your network to slowly adapt to the data intuitively makes sense. However, theoretically, the main reason for warmup steps is to allow adaptive optimisers (e.g. Adam, RMSProp, ...) to compute correct statistics of the gradients. Therefore, a warmup period makes little sense when training with plain SGD."  (Copied form Ref. https://datascience.stackexchange.com/questions/55991/in-the-context-of-deep-learning-what-is-training-warmup-steps)

![100k Learning Rate](https://user-images.githubusercontent.com/64815523/109389707-80dbee00-78db-11eb-8a0c-57886a4d214e.JPG)

How big was your training set (mb)? How many training lines did it contain?

- There were 2 training files - train.de (size = 710 M, lines = 4562012) and train.en (size = 636 M, lines = 4562012)

![Size](https://user-images.githubusercontent.com/64815523/109389228-aa941580-78d9-11eb-8b37-675146f728f4.JPG)

What are the files that a TF checkpoint is comprised of?

A TF model checkpoint consists of a few different files - index, meta and data. The .ckpt-meta contains the structure of the computation graph of the model. The .ckpt-data contains the values for all the variables, without the structure.

![Checkpoint](https://user-images.githubusercontent.com/64815523/109389256-d7e0c380-78d9-11eb-9c20-b92caf33b3d3.JPG)

How big is your resulting model checkpoint (mb)?

- I changed the checkpoint to 29998, so I took a number of checkpoints:  In total ~3.7 GB.
![Checkpoint Sizes](https://user-images.githubusercontent.com/64815523/109389648-30fd2700-78db-11eb-8d36-69642444b358.JPG)

Remember the definition of a "step". How long did an average step take?

Looking at the tail nohup file, the average time taken for a step was 1.282 seconds. Extrapolating that to the elapsed time: each training step takes 1.282 seconds, then the total time taken to train is 100k x 1.181 = 128200 seconds or roughly, 35.6 hours. This is inline with our observed training time of 35 hrs, 37min shown in graph below.

![100k Score](https://user-images.githubusercontent.com/64815523/109389428-8edd3f00-78da-11eb-9085-0ac2e9f9c738.JPG)

How does that correlate with the observed network utilization between nodes?

I think that as the network utilization increases between the nodes, the more data is being transferred and the more often parameter updates are happening.  If thats the case there should be faster step times.

Other Images:

![100k Score](https://user-images.githubusercontent.com/64815523/109389712-85080b80-78db-11eb-8b99-eef12d0b6b73.JPG)

