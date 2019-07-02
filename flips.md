checking/thinking about consecutive coin flips
have been reading at several places about 'law of small numbers' in context to the hot hand fallacy, basically it seems like they need to adjust the 'bias' when counting the hits and they didn't do it properly in the original study
but now I'm seeing a lot of wrong interpretation of what this means, so in order to get my thinking straight will try to analyze this using simulation (more like enumaration) to see if I understand their argument

first i will be looking at a fair coin, strings of 4 tosses so all scenarios are still tractable
this code will generte 2**4 possible realizations, all equally likely

```python
def streak2():
    n = 4
    nsims = 2**n
    total_heads, total_sums = 0, 0
    for i in range(nsims):
        heads, nums = 0, 0
        x = bin(i)[2:]
        while len(x) < n: x = '0'+ x
        print('\n',x)
        for k in range(n-1):
            if x[k] == '1':
                nums+=1
                total_sums += 1
                if x[k+1] == '1':
                    heads += 1
                    total_heads += 1
        print('%.0f heads in %.0f chances'%(heads, nums))
        if nums > 0:
            print('%.2f success'%(heads/nums*100))
```

so here are the results:

```
 0000	0 heads in 0 chances	
 0001	0 heads in 0 chances	
 0010	0 heads in 1 chances	  0.00 success	
 0011	1 heads in 1 chances	100.00 success	
 0100	0 heads in 1 chances	  0.00 success	
 0101	0 heads in 1 chances	  0.00 success	
 0110	1 heads in 2 chances	 50.00 success	
 0111	2 heads in 2 chances	100.00 success	
 1000	0 heads in 1 chances	  0.00 success	
 1001	0 heads in 1 chances	  0.00 success	
 1010	0 heads in 2 chances	  0.00 success	
 1011	1 heads in 2 chances	 50.00 success	
 1100	1 heads in 2 chances	 50.00 success	
 1101	1 heads in 2 chances	 50.00 success	
 1110	2 heads in 3 chances	 66.67 success	
 1111	3 heads in 3 chances	100.00 success	
```
so the main question is given there is a heads, what are the odds that the following toss is also a heads. 
intuitevly its 'obvious' it should be 50%
looking at the 16 scenarios, the first 2 don't have any heads before the last toss so never have a chance of 2 consecutive heads. of the remaining 14, 6 have 0% success, 4 have 50%, one 66.67% and 3 have 100%.
so the argument seems to be the following: ignore first 2 (where there is no chance of having HH) and average all the other 14 as equally likely given each scenario has the same probability of happening
this will result in (0%*6 + 50%*4 + 66.67%*1 + 100%*3)/14 = 40 10/21% = 40.48%
if for some reason we decide to include the first 2 scenarios then we have 35 5/12% = 35.42%
 
