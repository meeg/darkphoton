/*******************************************************************************
* v1495Init - Initialize v1495 Library. 
*
* RETURNS: OK, or ERROR if the address is invalid or board is not present.
********************************************************************************/
STATUS v1495Init (UINT32 addr, UINT32 addr_inc, int nmod)

/*******************************************************************************
* v1495Status - The status of v1495
*
* RETURNS: prints some status of v1495 on screen
********************************************************************************/
void v1495Status (int id)

/*******************************************************************************
* v1495TimewindowSet - Set the internal TDC readout time window of v1495
*
* RETURNS: error if the input setting is not correct
********************************************************************************/
void v1495TimewindowSet (int id, int val)

/*******************************************************************************
* v1495TimewindowRead - readout the time window setting value
** RETURNS: setting value or error if the board is not initialized
********************************************************************************/
int v1495TimewindowRead (int id)

/*v1495 reset now can't work*/
void v1495Reset (int id)

void delay (int ms);

/*******************************************************************************
* v1495Timeset - Set the v1495 internal delay and jittering acceptable range
*
* RETURNS: setting value or error if the board is not initialized
********************************************************************************/
void v1495Timeset (int n, int id, int sleep)

//This function simply calls sprintf to fill a character string
//It was moved to its own function to prevent interference with nanosleep() -- REM -- 2013-09-13
void v1495sprintf(char* aString, int size, char aCharge, int fileNum, char fileType)

//Activate Pulser (This disables 'data-mode' pass-through of input channels)
void v1495ActivatePulser(int id0)

//Disactivate Pulser (This enables 'data-mode' pass-through of input channels)
void v1495DisactivatePulser(int id0)

/*******************************************************************************
* v1495PatternSet - Set the data for Level0 pulser (and Level2 pulser control)
*
* RETURNS: setting value or error if the board is not initialized
********************************************************************************/
void v1495PatternSet (int level, int id, char charge, char TorB, int patnum)

void v1495DelayTest(int sec, int nsec)

void v1495PulserTest(int id)

void v1495PulserGo(int id)

/*Reads TDC and pattern file, calculates expected and measured TDCtimes, compares, outputs result */
/*INCOMPLETE, MADE OBSOLETE BY v1495SimplePulser() and v1495RoadPulser() */
void v1495PulserCompare(int id)

//This function measures the 'Level 2' TDC hits in a 3-board pulser test configuration 
//Currently it only uses the first 10 roads (first ten lines) 
//THIS FUNCTION CONTAINS HARD CODED LEVEL 2 MAPPING
void v1495RoadPulser(int id0, int id1, int id2, char charge, const int numFiles, const int iter, int win0, int win1, int win2 )

/*This function measures the 'Level 1' TDC hits in a 3-board pulser test configuration */
/*It calculates the mean, min, and max of TDCtimes for each channel individually */
/*It should be used with a pattern file that has ffff,ffff,ffff,... in some rows, and all zeros in all other rows. */
void v1495SimplePulser(int id0, int id1, int id2, int win0, int win1, int win2, const int iter)

/* OUTDATED */
/* Activates the signal sent out from Level 2, to Level 0, in order to start the pulsers -- REM -- 2013-07-09 */
void v1495PulserOnSignal(int id)

/* Activates the signal sent out from Level 2, to G1 on Levels 1 and 2, to stop the TDCs -- REM -- 2013-07-18 */
void v1495tdcStopSignal(int id)

void v1495TimesetTest (int id, int sleep)

void v1495Chread (int id, int sleep)

void v1495Chout (int id)

void v1495Run (int id)

void dummydelay (int delay)

void v1495Reload (int id)

void v1495AutoTimeset (int id, int sleep, short seedch, int startp)

/*******************************************************************************
* v1495TDCRead - This function is to readout the data for debug !DON'T PUT IN .CRL!
* MAY CRASH ROC!!!
* RETURNS: TDC data
********************************************************************************/
void v1495TDCRead (int id)

int v1495TDCReadout (int id, int ii)

int v1495TDCcount (int id)

int v1495RevisionRead (int id)

int v1495CommonstopRead (int id)

int g1 (int id, int row)

void v1495ciptag (int id)

void v1495Writetest (int id, int round, int loopn)

void v1495Readtest (int id, int round, int loopn)

void v1495Readtest1 (int id, int round, int loopn)

void v1495Readtest2 (int id, int round, int loopn)

void v1495Readtest3 (int id, int round, int loopn)

void v1495ReadWritetest (int id, int round, int loopn)

//MAY NOT WORK AFTER TODAY - REM - 2013-03-14
void v1495Memwritese (int id, int mode, int loop, int base)

void v1495Memwriterd (int id, int mode, int loop, int loopn)

void v1495Memread (int id, int round, int loopn)

void v1495Memreadcon (int id, int round, int loopn)

void v1495Testread (int id)
