CXXFLAGS=-DRPI

all: send codesend RFSniffer
	
send: RCSwitch.o send.o
	$(CXX) $(CXXFLAGS) $(LDFLAGS) $+ -o ../build/$@ -lwiringPi

codesend: RCSwitch.o codesend.o
	$(CXX) $(CXXFLAGS) $(LDFLAGS) $+ -o ../build/$@ -lwiringPi
	
RFSniffer: RCSwitch.o RFSniffer.o
	$(CXX) $(CXXFLAGS) $(LDFLAGS) $+ -o ../build/$@ -lwiringPi

clean:
	$(RM) *.o send codesend RFSniffer
