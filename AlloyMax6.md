#alloy 


# Biggest differences between alloy 5 and 6


# Find the first diff

![[Pasted image 20250721130621.png]]

central.xml allows us to declare what sat solvers we wanted to add.

![[Pasted image 20250721131749.png]]

As we can see we added 
- sat4j core
- maxsat
- pb

# PROBLEM! No central.xml

![[Pasted image 20250721132019.png]]

According to [alloy's official page](https://alloytools.org/documentation/alloy-api/edu/mit/csail/sdg/alloy4compiler/translator/A4Options.html?utm_source=chatgpt.com) 

![[Pasted image 20250721161851.png]]

Which we can find in the Alloy 6 code in the following path 

![[Pasted image 20250721161916.png]]

# The Fix? 

Well lets look at this part of the A4Options

![[Pasted image 20250723150112.png]]
this tells us that we're using Factory Structures which is entirely different than the alloy5 version. 

Instead lets just come back to this. 


# Lets add the soft constraint system

![[Pasted image 20250723150756.png]]

this is the first section that mentions anything about soft constraints in the ast/Expr.java file. 

Alloy 6 version? 
![[Pasted image 20250723150901.png]]


Now lets make a new version of the code we found above. 

![[Pasted image 20250723153550.png]]

next we'll do this one 

![[Pasted image 20250723153645.png]]

Found where to add this code. 

![[Pasted image 20250724151416.png]]

Adding the following code 

![[Pasted image 20250724151755.png]]

- `maxsome` — a soft existential quantifier ("maximize how many this satisfies")
    
- `minsome` — a soft dual ("minimize how many this satisfies")

This tells the parser this is a new kind of quantified formula. 

Enabling users to write 

```
maxsome x: Person | x.likes[IceCream]
```

Which will then be compiled into an ExpUnary code
Then marked with Op.MAXSOME
and finally ready to be translated into a weighted MAXSat Clause


# Fixing the keywords 

seems like I overlooked some other edits. 

![[Pasted image 20250724152053.png]]

PreferencesDialog.java

Doesn't exist in Alloy 6-

WRONG. I wasted time on this.

It does exist, here. 


![[Pasted image 20250730161903.png]]

where we take the old code 

![[Pasted image 20250730162126.png]]

and add it here 

![[Pasted image 20250730162139.png]]

