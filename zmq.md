
```c++
#include<iostream>
#include<string>
#include<cassert>
#include<zmq.hpp>
#include<unistd.h>

using namespace std;

int main() {

    const auto thread_num = 1;

    zmq::context_t context(thread_num);

    auto make_sock = [&](auto mode) {
        return zmq::socket_t(context, mode);
    };

    auto addr = "ipc:///dev/shm/zmq.sock"s;

    auto receiver = [=](){
        auto sock = make_sock(ZMQ_PULL);

        sock.bind(addr);
        assert(sock.connected());

        zmq::message_t msg;
        sock.recv(&msg);

        string s = {msg.data<char>(), msg.size()};
        cout << s << endl;
    };

    auto sender = [=]() {
        auto sock = make_sock(ZMQ_PUSH);
        sock.connect(addr);
        assert(sock.connected());

        string s = "hello zmq";
        sock.send(s.data(), s.size());
    };

    auto fpid = fork();
    if(fpid < 0) {
        assert(false);
    }
    else if(fpid == 0) {
        sender();
    }
    else {
        receiver();
    }

    return 0;
}
```
