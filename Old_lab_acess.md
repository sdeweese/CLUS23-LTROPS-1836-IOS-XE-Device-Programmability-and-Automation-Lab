# OLD LAB ACCESS :)
1. Identify your pod#.
 
2. Open two terminal windows and SSH to both. One window will be used to configure the VM. The second window is for telnet access into the C9300.

3. To SSH into the VM, copy/paste the below line into **the first** terminal sessions. Replace the ## symbol on the SSH command with your pod number. Use the password given to you by the facilitator.

```ssh -p 3389 -L 18480:localhost:8480 -L 13000:localhost:3000 auto@pod##-xelab.cisco.com```

Once you logged into the VM, the first time you login, you'll see this question:

`Are you sure you want to continue connecting (yes/no/[fingerprint])?` 

Type, `yes` to continue 

After you approve the entry you should be able to see the following prompt:

![](./imgs/pod_login.PNG)


4. Similarly, in the second terminal window, run the following command. Replace the ## symbol on the SSH command with your pod number. Use the password is the same one that was given to you by the facilitator in the previous step.

```auto@pod##-xelab.cisco.com```

Next, telnet into the Catalyst 9300 in the same window. 

```telnet c9300```

Use the following credentials: admin / Cisco123

![](./imgs/switch_login.PNG)

5. Once you finished accesing via SSH and telnet into the VM and the switch respectively, this is how you should see them:

![](./imgs/vm_c9300_terminals.PNG)