        #include <iostream>
    #include <ctime>
     #include <thread>
      #include <chrono>

      using namespace std;

        class Alarm {
        private:
    time_t alarmTime;
    bool isSet;

        public:
    Alarm() : isSet(false) {}

    void setAlarm(time_t time) {
        alarmTime = time;
        isSet = true;
    }

    void checkAlarm() {
        while (true) {
            if (isSet && time(nullptr) >= alarmTime) {
                cout << "Alarm triggered!" << endl;
                break;
            }
            // Check every second
            this_thread::sleep_for(chrono::seconds(1));
        }
    }
    };

      int main() {
    Alarm alarm;

    // Set alarm for 10 seconds from now
    time_t currentTime = time(nullptr);
    time_t alarmTime = currentTime + 10;
    alarm.setAlarm(alarmTime);

    // Start checking for alarm trigger
    thread alarmThread(&Alarm::checkAlarm, &alarm);
    alarmThread.join();

    return 0;
    }
