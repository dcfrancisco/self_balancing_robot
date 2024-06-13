If your robot is oscillating back and forth about an inch, it could be due to an issue with the PID control parameters. Here are a few steps to help you troubleshoot and adjust the PID values:

### Steps to Adjust PID Values

1. **Increase the Proportional Gain (`kp`)**: If `kp` is too low, the system might not respond aggressively enough to correct the position, leading to oscillations.
   
   ```cpp
   double kp = 25; // Increase this value slightly
   ```

2. **Decrease the Integral Gain (`ki`)**: If `ki` is too high, it can cause overshooting and oscillations. Start with `ki` set to zero and gradually increase it if necessary.
   
   ```cpp
   double ki = 0.0; // Start with 0 and gradually increase if necessary
   ```

3. **Adjust the Derivative Gain (`kd`)**: `kd` helps to dampen the oscillations. If it's too low, the system won't be able to react quickly to changes, leading to oscillations. Increase `kd` gradually to see if it helps.
   
   ```cpp
   double kd = 0.6; // Increase this value slightly
   ```

### Testing Adjustments

1. **Initial Testing**: 
   - Start with the default values and make small adjustments. 
   - Increase `kp` incrementally to see if it reduces oscillations.
   - If oscillations persist, increase `kd` gradually to add more damping.
   - Keep `ki` at zero initially.

2. **Incremental Changes**: Make small changes to the PID values and test the robot's behavior after each change.

3. **Monitor Responses**: Observe the robot's movement closely after each adjustment. Look for changes in the oscillation pattern and stability.

Here is how you can update the PID values in your code:

```cpp
// Current PID values
double kp = 23, ki = 0.0, kd = 0.48; // Angle loop PD control parameters

// Adjusted PID values
double kp = 25, ki = 0.0, kd = 0.6; // Updated values based on testing
```

### Example of Adjusting the Angle Loop PD Control
In your `angleout()` function, you apply the PID control for the angle:

```cpp
void angleout()
{
    balancecar.angleoutput = kp * (kalmanfilter.angle + angle0) + kd * kalmanfilter.Gyro_x; // PD Angle loop control
}
```

After making these adjustments, re-upload the code to your robot and observe the changes in its behavior. Keep iterating on these values until the robot maintains a stable position without significant oscillations.