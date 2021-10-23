# LDU(Log-based Deferred Update)

We propose a novel light weight concurrent update method, LDU, to improve performance scalability for Linux kernel on many-core systems through eliminating lock contentions for update-heavy global data structures during process spawning and optimizing update logs.

## Information

The proposed LDU is implemented into Linux kernel 4.5-rc6 and evaluated using representative benchmark programs. Our evaluation reveals that the Linux kernel with LDU shows performance improvement by ranging from 1.2x through 2.2x on a 120 core system. 

## License

It is distributed under the GNU General Public License - see the accompanying COPYING file for more details. 

You can find more detail in here(https://www.actapress.com/Abstract.aspx?paperId=456169).

## Usage

If you want to use this kernel, you can usually replace it by using general kernel compilation method.

### Build

```bash
 make -jx && make modules -jx && make modules_install -jx INSTALL_MOD_STRIP=1 && make install -jx
```

```
x = number of core (recommended)
```

## Developer guide

Challenges to designing a deferred update mechanism includes performing concurrent update with minimal cache line transfers allowing parallel updates. At each update operation, LDU records this update operation log to lock-less list. Before the read operation, LDU applies the updates log in chronological order. In order to deferred update, LDU divide the update operation into logical update and physical update. The logical update inserts logs into the lock-less list and carries out update side absorbing; on the other hand, the physical update executes these operations that are minimized by the update side absorbing.

## Result

### File reverse mapping data structure

<img width="447" alt="LDU file reverse mapping " src="https://user-images.githubusercontent.com/28583545/138095367-11e2e3f6-1fc2-4760-838a-9d1bfdbfdbe0.png">

### Anonymous reverse mapping data structure

<img width="473" alt="LDU anonymous reverse mapping" src="https://user-images.githubusercontent.com/28583545/138095419-8924490b-8d35-4945-a68e-5044ddb50d0d.png">

### Running LDU example

<img width="451" alt="Running LDU example" src="https://user-images.githubusercontent.com/28583545/138095478-5959a7ba-f1d2-4423-afe2-8650a21d42ea.png">

### Stock Linux benchmark result

<img width="631" alt="Stock linux benchmark result" src="https://user-images.githubusercontent.com/28583545/138094897-553ac15a-bcd5-4872-a204-f5487b781d9c.png">

### AIM7 multiuser benchmark result of Stock Linux and Linux applied LDU

<img width="594" alt="Stock linux and LDU linux AIM7 benchmark result" src="https://user-images.githubusercontent.com/28583545/138095535-b7b5d05d-b577-4a82-8565-15e6ba1097f0.png">

## Publication

Joohyun Kyong and Sung-Soo Lim, LDU: A Lightweight Concurrent Update Method with Deferred Processing for Linux Kernel Scalability
