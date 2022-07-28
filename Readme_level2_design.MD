# Challenges-arka-23
Challenges-arka-23 created by GitHub Classroom
# Bitmanipulation Coprocessor Design Verification

The verification environment is setup using [Vyoma's UpTickPro](https://vyomasystems.com) provided for the hackathon.

![image](https://user-images.githubusercontent.com/70422874/180929690-d6604e02-f38c-4fe6-9dc7-b5f2b4bb003a.png)

## Verification Environment

The [CoCoTb](https://www.cocotb.org/) based Python test is developed as explained. The test drives inputs to the Design Under Test (Bitmanipulation Coprocessor module here) which takes in 3 8-bit inputs *mav_putvalue_src1*, *mav_putvalue_src2*, *mav_putvalue_src3*, and a 32-bit input *mav_putvalue_instr* synchronized to a clock, a 1-bit *RST_N* reset signal and a 1-bit *EN_mav_putvalue* enable input, and finally gives a 8-bit output as *mav_putvalue*.

The values are assigned to the input port using 
```
     X = random.randint(0, 2)
        for _ in range(5):
            # reset
            dut.RST_N.value <= 0
            yield Timer(10) 
            dut.RST_N.value <= 1

            ######### CTB : Modify the test to expose the bug #############
            # input transaction
            mav_putvalue_src1 = random.randint(0, 15)
            mav_putvalue_src2 = random.randint(0, 15)
            mav_putvalue_src3 = random.randint(0, 15)        
            mav_putvalue_instr = 0x400170B3 + X

            # expected output from the model
            expected_mav_putvalue = bitmanip(mav_putvalue_instr, mav_putvalue_src1, mav_putvalue_src2, mav_putvalue_src3)

            # driving the input transaction
            dut.mav_putvalue_src1.value = mav_putvalue_src1
            dut.mav_putvalue_src2.value = mav_putvalue_src2
            dut.mav_putvalue_src3.value = mav_putvalue_src3
            dut.EN_mav_putvalue.value = 1
            dut.mav_putvalue_instr.value = mav_putvalue_instr
    
```

![image](https://user-images.githubusercontent.com/70422874/181428871-0516733a-d9f9-4f55-9d78-aa5fa7371886.png)


The assert statement is used for comparing the MUX's outut to the expected value.

## The following error is seen:
```
 AssertionError: Value mismatch DUT = 0x2 does not match MODEL = 0x0. Instruction not present, expected output is 0x0
 ```
![image](https://user-images.githubusercontent.com/70422874/181428287-164b7f59-f397-46c3-af88-2152c621a1ae.png)
 
## Test Scenario **(Important)**
X = random.randint(0, 2)
- mav_putvalue_src1 = random.randint(0, 15)
- mav_putvalue_src2 = random.randint(0, 15)
- mav_putvalue_src3 = random.randint(0, 15)
- mav_putvalue_instr = 0x400170B3 + X
- Expected Output: expected_mav_putvalue = 0x0
- Observed Output in the DUT: dut_output = 0x2

Output mismatches for the above inputs proving that there is a design bug

## Design Bug
Based on the above test input and analysing the design, we see the following

```
Instruction not present, expected output is 0x0
```

## The following error is seen:
```
 AssertionError: Value mismatch DUT = 0x3 does not match MODEL = 0x5. Logic error in ANDN 1 block
 ```
![image](https://user-images.githubusercontent.com/70422874/181428644-840c7933-5a17-4107-8972-7cf132a6b6b6.png)
 
## Test Scenario **(Important)**
X = random.randint(0, 2)
- mav_putvalue_src1 = random.randint(0, 15)
- mav_putvalue_src2 = random.randint(0, 15)
- mav_putvalue_src3 = random.randint(0, 15)
- mav_putvalue_instr = 0x400170B3 + X
- Expected Output: expected_mav_putvalue = 0x5
- Observed Output in the DUT: dut_output = 0x3

Output mismatches for the above inputs proving that there is a design bug

## Design Bug
Based on the above test input and analysing the design, we see the following

```
 Logic error in ANDN 1 block
```

## Design Fix
Updating the design and re-running the test makes the test pass.


## Verification Strategy

## Is the verification complete ?
