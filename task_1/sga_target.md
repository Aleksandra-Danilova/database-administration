### Problem
Increase SGA_TARGET by 2 times in SCOPE=SPFILE.

### Solution
Let's look at the values of SGA_TARGET and SGA_MAX_SIZE: initially they are equal to 404M.

![image](https://user-images.githubusercontent.com/76550825/166064540-57d13261-52f9-44c9-8029-eb1b9462ced5.png)

Increase the value of SGA_MAX_SIZE to 808M.

![image](https://user-images.githubusercontent.com/76550825/166064629-04750e42-b8d1-4ed1-82b8-bbeb4639f0e2.png)

Then increase the size of SGA_TARGET to 808M too.

![image](https://user-images.githubusercontent.com/76550825/166064833-158b98d8-6db0-44bf-b043-33d207224de3.png)

Since we are working with the scope value equal to SPFILE, we perform a restart using SHUTDOWN IMMEDIATE and STARTUP.

![image](https://user-images.githubusercontent.com/76550825/166064941-cb531c69-99a4-444e-8762-df89f1453f63.png)

Check that the new parameter values have taken effect.

![image](https://user-images.githubusercontent.com/76550825/166065055-8b17b68f-2be8-403a-814e-5d4cf4fcd24e.png)
