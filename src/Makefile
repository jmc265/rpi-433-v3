CXXFLAGS=-DRPI

all: send codesend RFSniffer
	
send: RCSwitch.o send.o
	$(CXX) $(CXXFLAGS) $(LDFLAGS) $+ -o ../build/$@ -lwiringPi -lpthread

codesend: RCSwitch.o codesend.o
	$(CXX) $(CXXFLAGS) $(LDFLAGS) $+ -o ../build/$@ -lwiringPi -lpthread
	
RFSniffer: RCSwitch.o RFSniffer.o
	$(CXX) $(CXXFLAGS) $(LDFLAGS) $+ -o ../build/$@ -lwiringPi -lpthread

clean:
	$(RM) *.o send codesend RFSniffer
