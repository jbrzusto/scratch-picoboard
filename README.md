# Using a Sparkfun PicoBoard with Scratch 3

2020-03-06 **NOT YET WORKING**: my scratch-vm fork on github is missing a commit; will
fix this evening.

The hard work was already done here: https://github.com/audetto/asi-link
That repo provides a replacement for ScratchLink that serves up local
serial ports on a websocket server so that a local instance of Scratch 3
can connect to them.  The PicoBoard connects to your computer as a USB serial device.

Tiny changes to that, along with small changes to 3 of the official Scratch repos
let you run an instance of scratch-gui that will be able to use a PicoBoard plugged
into your computer.

This has only beedn tested on linux Ubuntu 18.04 so far.

## Installing (Ubuntu 18.04) from bash shell

```
# install node, npm, and git; skip if you have these
sudo apt-get install nodejs npm git p7zip-full

# make a directory and clone needed repos
export PICO=/home/$USER/picoboard
mkdir $PICO
cd $PICO
for r in scratch-picoboard scratch-vm scratch-gui scratch-blocks asi-link; do
    git clone --depth=1 picoboard https://github.com/jbrzusto/$r
    cd $r
    git checkout picoboard
    cd ..
done

# follow the standard instructions for installing scratch 3 locally
# for me, npm-link only works with 'sudo'; your installation might not need it
cd asi-link
# not sure what these are used for, TBH
. ./get_certificates.sh
npm install
cd ../scratch-vm
npm install
sudo npm link
cd ../scratch-blocks
npm install
sudo npm link
cd ../scratch-gui
npm install
sudo npm link ../scratch-vm
sudo npm link ../scratch-blocks
npm run build
```


## Running (Ubuntu 18.04) from bash shell
```
# run the serial-port scratchlink replacement
cd $PICO/asi-link
node app.js &
#
