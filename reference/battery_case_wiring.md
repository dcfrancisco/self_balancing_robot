
3s Battery Case Wiring
```

  +---(B1+)--------(B1-)---+---(B2+)--------(B2-)---+---(B3+)--------(B3-)---+
  |                       Wire 2                    Wire 3                  |
 Wire 1                   |                           |                     |
  |                       |                           |                     |
  |                       |                           |                     |
  +-----------------------+---------------------------+---------------------+
                                                                            
  Balance Connector:                                                        
  +---+---+---+---+                                                       
  | 1 | 2 | 3 | 4 |                                                       
  +---+---+---+---+                                                       
  |   |   |   |   |                                                        
  |   |   |   |   +--- B3 Positive Terminal                                
  |   |   |   +------- Between B2 and B3                                   
  |   |   +----------- Between B1 and B2                                   
  |   +--------------- B1 Negative Terminal                                
  +-----------------------------------------------------------------------+                                 

```

Wiring Instructions for Converting Battery Case into Balance Charging Case:

To convert a battery case into a balance charging case, follow these wiring instructions:

1. Cut the wires of the battery case, leaving enough length to work with.
2. Strip the insulation from the ends of the wires.
3. Connect the positive terminal of the balance connector (pin 4) to the positive wire of the battery case (Wire 3).
4. Connect the negative terminal of the balance connector (pin 1) to the negative wire of the battery case (Wire 1).
5. Connect the middle terminal of the balance connector (pin 2) to the wire between B1 and B2.
6. Connect the remaining terminal of the balance connector (pin 3) to the wire between B2 and B3.

Once you have completed these wiring instructions, your battery case will be converted into a balance charging case, allowing you to charge and balance the individual cells of your battery pack.


Certainly! Here’s an ASCII circuit diagram for connecting 3x 18650 batteries in series to create a 3S battery pack with balance leads:

```
  +---(B1+)--------(B1-)---+---(B2+)--------(B2-)---+---(B3+)--------(B3-)---+
  |                       Wire 2                    Wire 3                  |
 Wire 1                   |                           |                     |
  |                       |                           |                     |
  |                       |                           |                     |
  +-----------------------+---------------------------+---------------------+
                                                                            
  Balance Connector:                                                        
  +---+---+---+---+                                                       
  | 1 | 2 | 3 | 4 |                                                       
  +---+---+---+---+                                                       
  |   |   |   |   |                                                        
  |   |   |   |   +--- B3 Positive Terminal                                
  |   |   |   +------- Between B2 and B3                                   
  |   |   +----------- Between B1 and B2                                   
  |   +--------------- B1 Negative Terminal                                
  +-----------------------------------------------------------------------+                                 
```

### Explanation:
1. **Battery Connections:**
   - B1: First battery
   - B2: Second battery
   - B3: Third battery

2. **Wires:**
   - **Wire 1:** Connects to the negative terminal of B1.
   - **Wire 2:** Connects between the negative terminal of B1 and the positive terminal of B2.
   - **Wire 3:** Connects between the negative terminal of B2 and the positive terminal of B3.

3. **Balance Connector:**
   - **Pin 1:** Connected to Wire 1 (B1 Negative Terminal)
   - **Pin 2:** Connected to Wire 2 (Between B1 and B2)
   - **Pin 3:** Connected to Wire 3 (Between B2 and B3)
   - **Pin 4:** Connected to the positive terminal of B3

Ensure the wiring is correct and secure before connecting it to your balance charger. This will help in balancing the individual cells during the charging process.