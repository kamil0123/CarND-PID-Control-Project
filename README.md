# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---
### Result video

https://www.youtube.com/watch?v=H28e3MH8kkg

## Reflections

### Describe the effect each of the P, I, D components had in your implementation.

#### P
The P ("proportional") component has proportional effect to the current value of crosstrack error (CTE).
With increase of Kp coefficient, car more rapidly is going back to the center of road. However, overshooting is also increasing.
If it's too low, the control action may be too small when responding to sharp turns of road.

#### I
The I ("integral") component is proportional to both the magnitude of the error and the duration of the error (sum of all CTE). It removes systematic bias.
Increasing of Ki coefficient gives less bias, but if it'll be too big it can cause instability. 

#### D
The D ("differential") component neutralize the P component's tendency to overshooting.
With good Kd coefficient car goes to center line smoothy. However, when Kp is too big, car may not react in enought time on turns.

### Describe how the final hyperparameters were chosen.

I tuned parameteres manually. First I had plan to optimize them with twiddle algorithm, but then I realized that it will take a lot of  time, and manually choosen values gives better than expected results. 

1. I started with values, which were showed as example during course, Kp=0.2, Ki=0.004, Kd=3.0.
With this values car goes the whole lap, but it goes out the track for few seconds on sharp turns.
2. To find beter values I assumed I will change all values to zero, and will be tunig each paramater seperatlly.
3. Starting with Kp, I first check results with value 0.2, then compare it with 0.1 and 0.3. During all atemps car is going on almost straight road and it's tryng to go perfectly center by taking little turns from left to right, but overshooting is increasing (it makes bigger and bigger turns) and finally goes out the track. 
0.1 has the best results. Overshooting is increasing slower then with other two. Now I try Kp=0.15. Overshooting is just a little bigger than with 0.1, but I believe it will be better during turns. I choose Kp=0.15. 
4. With Kp=0.15, now I'm going to tune Ki. I try 0.004, than compare it with 0.002, 0.001, 0.00075. The car looks all the time more unstable than with Ki=0. The smaller value I choose it looks more stable. I don't want to take it too low, I'm choosing 0.00075 and I'm going to next parameter.
5. Now I have Kp=0.15, Ki=0.00075 and Kd=0. It's time to find the best Kd. I start with 3.0. Now car is starting to go well. It goes smoothly, but have problems during turns - it goes off the road. That means I need to get lower Kd. I try to find value with which car will drive calmly, but will react correctly on turns. I think Kd=1.0 gives good results.
6. My final results are Kp=0.15, Ki=0.00075, Kd=1.0.


## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./
