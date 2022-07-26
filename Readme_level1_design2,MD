# Challenges-arka-23
Challenges-arka-23 created by GitHub Classroom
# Sequence Detector Design Verification

The verification environment is setup using [Vyoma's UpTickPro](https://vyomasystems.com) provided for the hackathon.

![image](https://user-images.githubusercontent.com/70422874/180948964-27e570b1-0ec6-42bc-af2d-a9ff4168207b.png)

## Verification Environment

The [CoCoTb](https://www.cocotb.org/) based Python test is developed as explained. The test drives inputs to the Design Under Test (Sequence Detector module here) which takes in 1-bit inputs *inp_bit* as a sequence synchronized to a clock, a 1-bit *reset* and gives 1-bit output *seq_seen*

The values are assigned to the input port using 
```
     for i in range(2):
        bit = random.randint(0,1)
        dut.inp_bit.value = bit  
        await FallingEdge(dut.clk)
        
    dut.inp_bit.value = 1
    await FallingEdge(dut.clk)
    dut.inp_bit.value = 0
    await FallingEdge(dut.clk)
    dut.inp_bit.value = 1
    await FallingEdge(dut.clk)
    dut.inp_bit.value = 1   
```
![image](https://user-images.githubusercontent.com/70422874/180702258-0cc62be4-44c0-4bba-8946-211711a2d0c7.png)


The assert statement is used for comparing the MUX's outut to the expected value.

## The following error is seen:
```
 AssertionError: Randomised test failed with: sequence - [1, 0, 1, 0, 1, 1]. Error in case SEQ_101, where if input bit is 0, next_state = SEQ_10
 ```
![image](https://user-images.githubusercontent.com/70422874/180706148-a7f4a163-e191-4496-ab6b-4658d6147896.png)
 
## Test Scenario **(Important)**
- Test Inputs: inp_bit sequence = 101011
- Expected Output: A = 1
- Observed Output in the DUT dut.seq_seen.value = 0

Output mismatches for the above inputs proving that there is a design bug

## Design Bug
Based on the above test input and analysing the design, we see the following

```
Error in case SEQ_101, where if input bit is 0, next_state = SEQ_10
```


## The following error is seen:
```
  AssertionError: Randomised test failed with: sequence - [0, 1, 1, 0, 1, 1]. Error in case SEQ_1, where if input bit is 1, next_state = SEQ_1
```
![image](https://user-images.githubusercontent.com/70422874/180706426-cad87293-08a5-4c54-bd69-76d73a62e7d9.png)

## Test Scenario **(Important)**
- Test Inputs: inp_bit sequence = 011011
- Expected Output: A = 1
- Observed Output in the DUT dut.seq_seen.value = 0

Output mismatches for the above inputs proving that there is a design bug

## Design Bug
Based on the above test input and analysing the design, we see the following

```
Error in case SEQ_1, where if input bit is 1, next_state = SEQ_1
```

## Design Fix
Updating the design and re-running the test makes the test pass.

INPUT SEQUENCE - 011011

![image](https://user-images.githubusercontent.com/70422874/180706712-772027e3-8c5a-43d9-836c-4695a2bdd1de.png)

INPUT SEQUENCE - 101011

![image](https://user-images.githubusercontent.com/70422874/180706950-eb71a826-736e-4de8-afb3-69e716a41eaa.png)



## Verification Strategy

## Is the verification complete ?
