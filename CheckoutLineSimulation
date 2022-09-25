#include <iostream>
#include <queue>
#include <chrono>
#include <thread>
#include <io.h>
#include <string>
#include <fstream>


class Customer
{
public:

    Customer(int a, int b) {
        arrivalTime = a;
        customerServiceTime = b;
        timeWaited = 0;
    }

    int arrivalTime;
    int customerServiceTime;
    int timeWaited;

    int getArrivalTime() {
        return arrivalTime;
    }

};

void simPause(int t) {
    std::cout << "Simulation step: " << t << std::endl;
    using namespace std::this_thread;
    using namespace std::chrono;
    sleep_for(milliseconds(1));
}

bool customerArrived(double prob) {
    double rv = rand() / (double(RAND_MAX) + 1);
    return (rv < prob);
}

void increment(int& x, int a) {
    x = x + a;
}

int getWaitTime(Customer A, int c) {
    int waitTime = c - A.arrivalTime;
    return waitTime;
}

int main() {

    std::queue<Customer> customerQueue;
    srand(int(time(NULL)));
    int simTime = 2000;
    int serviceTime = 15;
    int maxLineSize = 5;
    int curCustomerTime = 0;
    int droppedCustomers = 0;
    int totalServed = 0;
    int totalLineSize = 0;
    int totalWaitTime = 0;
    

    //Simulation loop
    for (int i = 0; i < simTime; i++) {
        simPause(i);
        increment(totalLineSize, (int)customerQueue.size());

        if (customerArrived(0.1) && customerQueue.size() < maxLineSize) {
            std::cout << "Customer arrived." << std::endl;
            Customer* C = new Customer(i, serviceTime);
            customerQueue.push(*C);
            std::cout << "New customer has arrived at time: " << C->getArrivalTime() << std::endl;
            std::cout << "Queue size now at: " << customerQueue.size() << std::endl;
            totalServed++;

        }
        else if (customerArrived(0.1) && customerQueue.size() == maxLineSize) {
            std::cout << "Line at max size, customer dropped." << std::endl;
            droppedCustomers++;
        }

        if (!customerQueue.empty()) {
            customerQueue.front().customerServiceTime--;
        }
        if (!customerQueue.empty() && customerQueue.front().customerServiceTime == 0) {

            customerQueue.pop();
            std::cout << "Customer at front of line is finished." << std::endl << "New queue size is: " << customerQueue.size() << std::endl;
            if (!customerQueue.empty()) {
                customerQueue.front().timeWaited = getWaitTime(customerQueue.front(), i);
                std::cout << "Previous customer waited for: " << customerQueue.front().timeWaited << std::endl;
                increment(totalWaitTime, customerQueue.front().timeWaited);
            }

        }

    }

    double averageLineSize = (double)totalLineSize / simTime;
    double averageTimeWaited = (double)totalWaitTime / totalServed;
    std::cout << "Total customers served: " << totalServed << std::endl << "Total customers dropped: " << droppedCustomers << std::endl;
    std::cout << "Average line size: " << averageLineSize << std::endl;
    std::cout << "Average wait time for customer: " << averageTimeWaited << std::endl;

}
