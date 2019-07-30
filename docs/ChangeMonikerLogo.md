# Change Default Moniker logo

To change default Moniker logo you must have account on keybase. Perform the following setups to change logo.

## Create Account on Keybase

1. Visit  base.io/
2. create new account on it using sign up process.
3. Install keybase application on your linux machine.
4. Run following command to generate keybase pgp 
   ```
   keybase pgp gen 
   ```
this will generate add new pgp key in your account


## Updating your validator 

Now you have to update you validator with new pgp key you generated. 
Run the following  command to edit validator


```
colorcli stake edit-validator --identity="DA2034902BBED118" -–from=rnssol
```


