# Fixing Repeating Keys on Your Logitech Ergo Keyboard in Ubuntu

Lately, I was annoyed with the repeating keys when i write, for example if i
intend to write, 'hello' my keyboard would write 'hellllllloooo'. I was thinking
that it is temporary glitch due to *Bluetooth* disturbance or lower battery or
Power setting.

I was so annoyed and tried out all the things that came into mind but the 
problem wouldn't disappear. Finally, after some googling end up in a solution.

The problem is that my "Settings->Accessibility-> Repeat Keys and Delay" has
changed inadvertendly, i can't figure out how but i don't remember changing it.

I hope there is version of entire system configuration in repository, i guess
there must be something that would have been easier. But, that could be activity
for the future.

So, here is the outline of the solution.

## Adjust "Repeat Keys and Delay" Settings

Fine-tune the "Repeat Keys and Delay" settings in Ubuntu:

- Click on the Ubuntu settings icon in the top-right corner.
- Go to "Accessibility."
- Under "Typing," find the "Repeat Keys and Delay" settings.
- Use the "Delay" and "Speed" sliders to customize key repetition. Increase
  "Delay" for fewer repeats and adjust "Speed" to match your typing speed.
- Test your keyboard to ensure the issue is resolved.

## Bluetooth Driver Updates

Update your Bluetooth drivers:

```bash
sudo apt-get update
sudo apt-get install bluez bluez-tools
```        

## Power settings
Sometimes, enabling low power mode in settings could also affect so be sure to
check that it the problem persists. Also ensure the battery in the keyboard is
good enough.

If these didn't help, then go googling.

