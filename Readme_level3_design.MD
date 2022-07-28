
# Challenges-arka-23
Challenges-arka-23 created by GitHub Classroom
# Sequential Multiplier Design Verification

The verification environment is setup using [Vyoma's UpTickPro](https://vyomasystems.com) provided for the hackathon.

![image](https://user-images.githubusercontent.com/70422874/181436442-6e03e67f-4287-46d9-8114-0a6016856c60.png)

## Verification Environment

The [CoCoTb](https://www.cocotb.org/) based Python test is developed as explained. The test drives inputs to the Design Under Test (Sequence Detector module here) which takes in 1-bit inputs *inp_bit* as a sequence synchronized to a clock, a 1-bit *reset* and gives 1-bit output *seq_seen*

The values are assigned to the input port using 
```
    dut.RESET.value = 0
    await FallingEdge(dut.CLK)  
    dut.RESET.value = 1    
    a = random.randint(0, 1024)
    b = random.randint(0, 255)
    x = []
    y = []
    for i in range(11):  
        ######### CTB : Modify the test to expose the bug #############

        # input transaction       
        # driving the input transaction
        dut.in_Mx.value = a
        dut.in_My.value = b           
        await FallingEdge(dut.CLK) 

        # obtaining the output
        dut_output = dut.Prod.value        
       
        if i >= 1:
            if int(dut.Prod.value) != 0:
                x.append(bin(dut.Prod.value))
                y.append(dut_output)
        if i != 10:        
            cocotb.log.info(f'DUT OUTPUT={dut_output}')
        else:
            cocotb.log.info(f'DUT OUTPUT={dut_output}')
            cocotb.log.info(f'DUT DECIMAL OUTPUT={int(dut_output)}')

    
    # expected output from the model
    expected_value = a*b    
    cocotb.log.info(f'EXPECTED OUTPUT={expected_value}')

    temp1 = bin(a)   
    print("MULTIPLICAND : ", format(a, "b"))
    print("PRODUCT OUTPUT WHEN ACCUMULATOR IS LOADED WITH MULTIPLICAND FOR THE FIRST INSTANCE: ", y[0])
    
```

![image](https://user-images.githubusercontent.com/70422874/180946156-2e829f7e-5399-4bc6-a036-a8ccef9d14dd.png)

The assert statement is used for comparing the MUX's outut to the expected value.

## The following error is seen:
```
 AssertionError: Value mismatch DUT = 10404 does not match MODEL = 28560. Accumulator loaded with incorrect value, check Load-In block.
 ```
![image](https://user-images.githubusercontent.com/70422874/181436776-0d62b956-6ed0-4472-876f-8d6606188efb.png)
 
## Test Scenario **(Important)**
a = random.randint(0, 1024), b = random.randint(0, 255)
- dut.in_Mx.value = a
- dut.in_My.value = b       
- dut_output = dut.Prod.value 
- Expected Output: expected_value = 28560
- Observed Output in the DUT: dut_output = 10404

Output mismatches for the above inputs proving that there is a design bug

## Design Bug
Based on the above test input and analysing the design, we see the following

```
Accumulator loaded with incorrect value, check Load-In block.
```

## Design Fix
Updating the design and re-running the test makes the test pass.

![image](https://user-images.githubusercontent.com/70422874/181437014-81264a41-1c5c-4840-8414-90bb822a1d9e.png)

Error Introduced:

![image](https://user-images.githubusercontent.com/70422874/180947788-2dcc8aaa-c5a3-48c6-8c84-25d067d625a4.png)

Corrected Code:

![image](https://user-images.githubusercontent.com/70422874/180947906-caccbee7-4929-413b-8c41-6e438e26ec17.png)


## Verification Strategy

- DUT outputs observed thoroughly
- According to design, DUT output is the concatenation of the outputs of Accumulator and shift register
- According to design, after being reset the first value loaded to the Accumulator is the Multiplicand(other than 0) 
- Any variation in the above statement reflects that there might be an error in the loading steps

## Is the verification complete ?

Yes
