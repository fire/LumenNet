![Oculus VR, Inc.](RakNetLogo.jpg)\
\
  -------------------------------------------------------------------------
  ![](spacer.gif){width="8" height="1"}Memory Synchronizer Implementation
  -------------------------------------------------------------------------

+--------------------------------------------------------------------------+
| Memory Synchronizer Overview\                                            |
| \                                                                        |
| The memory synchronizer is an implementation of the message handler      |
| interface that scans memory for changes, serializes the changes, and     |
| sends it to specified systems. This is a more advanced implementation    |
| than in the DistribtedNetworkObject class.\                              |
| -   Can work with any topology, not just client/server.                  |
| -   Can specify target systems globally or per-data element, with        |
|     per-system synchronization                                           |
| -   Strings identify common pointers, rather than relying on the user to |
|     provide enumerations.                                                |
| -   Can use all send parameters to send updates.                         |
| -   Can include timestamps with updates.                                 |
|                                                                          |
| \                                                                        |
| Adding and removing participants is optional. Passing                    |
| UNASSIGNED\_PLAYER\_ID to either means all connected systems.            |
| Synchronizations will only be sent to and accepted by systems that are   |
| in the participant list. These functions are valid to call at any time.\ |
| void AddParticipant(PlayerID playerId);\                                 |
| void RemoveParticipant(PlayerID playerId);\                              |
| \                                                                        |
| SynchronizeMemory associates the pointer memory of size memorySize (in   |
| bytes) with a string identifier, or a class that implements MemSynchSpec |
| (see below). Pass 0 for either memory or memorySpec, since only one of   |
| these parameters is allowed. stringId must be a unique identifier for    |
| this memory block. If you have instantiated objects you either have to   |
| think of a scheme to provide a unique ID for each instance or else you   |
| can derive from NetworkIDGenerator and pass the value to                 |
| optionalInstanceID, which will concatenate that value to the string for  |
| you.\                                                                    |
| Priority, reliability, and ordering channel are equivalent to the        |
| parameters in the RakNet send call and are used in that capacity.\       |
| sendUpdates and receiveUpdates specify whether we scan for and send      |
| updates to other systems and whether we accept updates from other        |
| systems. Generally one system would send updates and all other systems   |
| receive updates for a particular memory block.\                          |
| void SynchronizeMemory(const char \*stringId, int maxUpdateFrequencyMS,  |
| PacketPriority priority, PacketReliability reliability, char             |
| orderingChannel, void \*memory, int memorySize, MemSynchSpec             |
| \*memorySpec, bool sendUpdates, bool receiveUpdates, ObjectID            |
| optionalInstanceId=UNASSIGNED\_OBJECT\_ID);\                             |
| \                                                                        |
| UnsynchronizeMemory stops synchronization of a memory block. Pass a      |
| value for either memory or memorySpec, and 0 for the unused parameter.\  |
| void UnsynchronizeMemory(void \*memory, MemSynchSpec \*memorySpec);\     |
+--------------------------------------------------------------------------+

  ---------------------------------------------------------
  ![](spacer.gif){width="8" height="1"}MemSynchSpec class
  ---------------------------------------------------------

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Custom memory synchronization per data element\
  \
  The class interface *MemSynchSpec* is used by the MemorySynchronizer implementation to provide the user with greater control over how and when memory elements are synchronized. To use, pass a pointer to an implementation of MemSynchSpec to the associated parameter in SynchronizeMemory and UnsynchronizeMemory.\
  You are supposed to serialize your networked data elements into the provided BitStream using the BitStream Write functions. lastSentValue and lastSendTime are provided in case you want to use them for optimization purposes.\
  virtual void Serialize(RakNet::BitStream \*bitstream, MemSynchSpec \*lastSentValue, unsigned long lastSendTime)=0;\
  \
  You are supposed to deserialize the data you serialized in the Serialize call. You can write the data to your networked memory directly, or elsewhere if you wish to interpolate to new values. timePacketSent will be a valid timestamped value (relative to your system) if IncludeTimestamp returns true.\
  virtual void Deserialize(RakNet::BitStream \*bitstream, unsigned int timePacketSent)=0;\
  \
  This is called when RakNet needs to make a copy of your networked data. You should allocate a copy of the class, copy your data to that class, and return the pointer.\
  virtual MemSynchSpec \* MakeSynchronizedDataCopy(void)=0;\
  \
  This is called when RakNet needs to write the last value it sent out. You should overwrite your networked data with the values held by the pointer. The pointer points to the user data that is synchronized, while the class called is the copy.\
  virtual void CopySynchronizedDataFrom(MemSynchSpec \*source)=0;\
  \
  RakNet no longer needs this copy of data. You can delete the class immediately if you wish via a call to *delete this;*\
  virtual Release(void)=0;\
  \
  This is an all-purpose update function telling RakNet if a given copy of data is different, different enough to send, and worth sending to a particular player. You have total control over whether the data gets sent or not - however for RakNet to properly record the last copy that was sent to each system IsUnifiedMemory must return true.\
  virtual bool ShouldSendUpdate(PlayerID destinationSystem, MemSynchSpec \*lastSentValue, unsigned long lastSendTime)=0;\
  \
  If you return true, the same value will be sent to all remote systems at the same time. This is useful for data that is always serialized to everyone the same way regardless of context, such as the current game score. If true, the same copy of the last sent data is used for all systems which saves memory and speed. You should return false if different destination systems will have different values sent to them or at different times. This should be used for contextual data, such as position updates which may not be sent to everyone, or may be sent more or less frequently. This will store an individual copy of the last sent data per remote system.\
  virtual bool IsUnifiedMemory(void)=0;\
  \
  Return true if you want a valid time value sent to Deserialize. Only set to true if you need it as bandwidth will be wasted otherwise.\
  virtual bool IncludeTimestamp(void)=0;\
  \
  For a usage example, see the sample *Samples\\Code Samples\\MemorySynchronization*.
  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  -----------------------------------------------
  ![](spacer.gif){width="8" height="1"}See Also
  -----------------------------------------------

  --------------------------------------------------------------
  [Index](index.html)\
  [Distributed Network Object](distributednetworkobject.html)\
  [Message Handler Interface](messagehandler.html)\
  [Fully Connected Mesh](fullyconnectedmesh.html)\
  --------------------------------------------------------------


