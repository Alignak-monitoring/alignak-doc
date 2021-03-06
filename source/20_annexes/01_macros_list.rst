.. _annexes/macros_list:

==========================
Standard Macros in Alignak
==========================


Standard macros that are available in Alignak are listed here. 

On-demand macros and macros for custom variables are described :ref:`here <monitoring_features/macros>`.


Macro Validity
==============

Although macros can be used in all commands you define, not all macros may be valid in a particular type of command. For example, some macros may only be valid during service notification commands, whereas other may only be valid during host check commands. 

Some commands are recognized and treated differently by Alignak. They are as follows:

    * Service checks
    * Service notifications
    * Host checks
    * Host notifications
    * Service :ref:`event handlers <monitoring_features/event_handlers>` and/or a global service event handler
    * Host :ref:`event handlers <monitoring_features/event_handlers>` and/or a global host event handler
    * Service performance data commands
    * Host performance data commands

The tables below list all macros currently available in Alignak, along with a brief description of each and the types of commands in which they are valid. If a macro is used in a command in which it is invalid, it is replaced with an empty string. It should be noted that macros consist of all uppercase characters and are enclosed in $ characters.


Macro Availability Chart
========================

Legend:


=== ==========================
No  The macro is not available
Yes The macro is available
=== ==========================


===================================================================================================================================== ============================================================ ============================================================ ============================================================ ============================================================ ================================================================================================================= ============================================================================================================== ================= =================================================================
Macro Name                                                                                                                            Service Checks                                               Service Notifications                                        Host Checks                                                  Host Notifications                                           Service Event Handlers                                                                                            Host Event Handlers                                                                                            Service Perf Data Host Perf Data
Host Macros: :ref:`03 <annexes/macros_list#note3>`
:ref:`$HOSTNAME$ <$HOSTNAME$>`                                                                                                        Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTDISPLAYNAME$ <$HOSTDISPLAYNAME$>`                                                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTALIAS$ <$HOSTALIAS$>`                                                                                                      Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTADDRESS$ <$HOSTADDRESS$>`                                                                                                  Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTSTATE$ <$HOSTSTATE$>`                                                                                                      Yes                                                          Yes                                                          Yes :ref:`01 <annexes/macros_list#note1>`                    Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTSTATEID$ <$HOSTSTATEID$>`                                                                                                  Yes                                                          Yes                                                          Yes :ref:`01 <annexes/macros_list#note1>`                    Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$LASTHOSTSTATE$ <$LASTHOSTSTATE$>`                                                                                              Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$LASTHOSTSTATEID$ <$LASTHOSTSTATEID$>`                                                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTSTATETYPE$ <$HOSTSTATETYPE$>`                                                                                              Yes                                                          Yes                                                          Yes :ref:`01 <annexes/macros_list#note1>`                    Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTATTEMPT$ <$HOSTATTEMPT$>`                                                                                                  Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$MAXHOSTATTEMPTS$ <$MAXHOSTATTEMPTS$>`                                                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTEVENTID$ <$HOSTEVENTID$>`                                                                                                  Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$LASTHOSTEVENTID$ <$LASTHOSTEVENTID$>`                                                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTPROBLEMID$ <$HOSTPROBLEMID$>`                                                                                              Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$LASTHOSTPROBLEMID$ <$LASTHOSTPROBLEMID$>`                                                                                      Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTLATENCY$ <$HOSTLATENCY$>`                                                                                                  Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTEXECUTIONTIME$ <$HOSTEXECUTIONTIME$>`                                                                                      Yes                                                          Yes                                                          Yes :ref:`01 <annexes/macros_list#note1>`                    Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTDURATION$ <$HOSTDURATION$>`                                                                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTDURATIONSEC$ <$HOSTDURATIONSEC$>`                                                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTDOWNTIME$ <$HOSTDOWNTIME$>`                                                                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTPERCENTCHANGE$ <$HOSTPERCENTCHANGE$>`                                                                                      Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTGROUPNAME$ <$HOSTGROUPNAME$>`                                                                                              Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTGROUPNAMES$ <$HOSTGROUPNAMES$>`                                                                                            Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$LASTHOSTCHECK$ <$LASTHOSTCHECK$>`                                                                                              Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$LASTHOSTSTATECHANGE$ <$LASTHOSTSTATECHANGE$>`                                                                                  Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$LASTHOSTUP$ <$LASTHOSTUP$>`                                                                                                    Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$LASTHOSTDOWN$ <$LASTHOSTDOWN$>`                                                                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$LASTHOSTUNREACHABLE$ <$LASTHOSTUNREACHABLE$>`                                                                                  Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTOUTPUT$ <$HOSTOUTPUT$>`                                                                                                    Yes                                                          Yes                                                          Yes :ref:`01 <annexes/macros_list#note1>`                    Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$LONGHOSTOUTPUT$ <$LONGHOSTOUTPUT$>`                                                                                            Yes                                                          Yes                                                          Yes :ref:`01 <annexes/macros_list#note1>`                    Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTPERFDATA$ <$HOSTPERFDATA$>`                                                                                                Yes                                                          Yes                                                          Yes :ref:`01 <annexes/macros_list#note1>`                    Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTCHECKCOMMAND$ <$HOSTCHECKCOMMAND$>`                                                                                        Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTACKAUTHOR$ <$HOSTACKAUTHOR$>` :ref:`08 <annexes/macros_list#note8>`                                                        No                                                           No                                                           No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$HOSTACKAUTHORNAME$ <$HOSTACKAUTHORNAME$>` :ref:`08 <annexes/macros_list#note8>`                                                No                                                           No                                                           No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$HOSTACKAUTHORALIAS$ <$HOSTACKAUTHORALIAS$>` :ref:`08 <annexes/macros_list#note8>`                                              No                                                           No                                                           No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$HOSTACKCOMMENT$ <$HOSTACKCOMMENT$>` :ref:`08 <annexes/macros_list#note8>`                                                      No                                                           No                                                           No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$HOSTACTIONURL$ <$HOSTACTIONURL$>`                                                                                              Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTNOTESURL$ <$HOSTNOTESURL$>`                                                                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTNOTES$ <$HOSTNOTES$>`                                                                                                      Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTBUSINESSIMPACT$ <$HOSTBUSINESSIMPACT$>`                                                                                    Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALHOSTSERVICES$ <$TOTALHOSTSERVICES$>`                                                                                      Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALHOSTSERVICESOK$ <$TOTALHOSTSERVICESOK$>`                                                                                  Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALHOSTSERVICESWARNING$ <$TOTALHOSTSERVICESWARNING$>`                                                                        Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALHOSTSERVICESUNKNOWN$ <$TOTALHOSTSERVICESUNKNOWN$>`                                                                        Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALHOSTSERVICESCRITICAL$ <$TOTALHOSTSERVICESCRITICAL$>`                                                                      Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes

Macro Name                                                                                                                            Service Checks                                               Service Notifications                                        Host Checks                                                  Host Notifications                                           Service Event Handlers                                                                                            Host Event Handlers                                                                                            Service Perf Data Host Perf Data
Host Group Macros:
:ref:`$HOSTGROUPALIAS$ <$HOSTGROUPALIAS$>` :ref:`05 <annexes/macros_list#note5>`                                                      Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTGROUPMEMBERS$ <$HOSTGROUPMEMBERS$>` :ref:`05 <annexes/macros_list#note5>`                                                  Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTGROUPNOTES$ <$HOSTGROUPNOTES$>` :ref:`05 <annexes/macros_list#note5>`                                                      Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTGROUPNOTESURL$ <$HOSTGROUPNOTESURL$>` :ref:`05 <annexes/macros_list#note5>`                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTGROUPACTIONURL$ <$HOSTGROUPACTIONURL$>` :ref:`05 <annexes/macros_list#note5>`                                              Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes

Macro Name                                                                                                                            Service Checks                                               Service Notifications                                        Host Checks                                                  Host Notifications                                           Service Event Handlers                                                                                            Host Event Handlers                                                                                            Service Perf Data Host Perf Data
Service Macros:
:ref:`$SERVICEDESC$ <$SERVICEDESC$>`                                                                                                  Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEDISPLAYNAME$ <$SERVICEDISPLAYNAME$>`                                                                                    Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICESTATE$ <$SERVICESTATE$>`                                                                                                Yes :ref:`02 <annexes/macros_list#note2>`                    Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICESTATEID$ <$SERVICESTATEID$>`                                                                                            Yes :ref:`02 <annexes/macros_list#note2>`                    Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$LASTSERVICESTATE$ <$LASTSERVICESTATE$>`                                                                                        Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$LASTSERVICESTATEID$ <$LASTSERVICESTATEID$>`                                                                                    Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICESTATETYPE$ <$SERVICESTATETYPE$>`                                                                                        Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEATTEMPT$ <$SERVICEATTEMPT$>`                                                                                            Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$MAXSERVICEATTEMPTS$ <$MAXSERVICEATTEMPTS$>`                                                                                    Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEISVOLATILE$ <$SERVICEISVOLATILE$>`                                                                                      Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEEVENTID$ <$SERVICEEVENTID$>`                                                                                            Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$LASTSERVICEEVENTID$ <$LASTSERVICEEVENTID$>`                                                                                    Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEPROBLEMID$ <$SERVICEPROBLEMID$>`                                                                                        Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$LASTSERVICEPROBLEMID$ <$LASTSERVICEPROBLEMID$>`                                                                                Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICELATENCY$ <$SERVICELATENCY$>`                                                                                            Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEEXECUTIONTIME$ <$SERVICEEXECUTIONTIME$>`                                                                                Yes :ref:`02 <annexes/macros_list#note2>`                    Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEDURATION$ <$SERVICEDURATION$>`                                                                                          Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEDURATIONSEC$ <$SERVICEDURATIONSEC$>`                                                                                    Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEDOWNTIME$ <$SERVICEDOWNTIME$>`                                                                                          Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEPERCENTCHANGE$ <$SERVICEPERCENTCHANGE$>`                                                                                Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEGROUPNAME$ <$SERVICEGROUPNAME$>`                                                                                        Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEGROUPNAMES$ <$SERVICEGROUPNAMES$>`                                                                                      Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$LASTSERVICECHECK$ <$LASTSERVICECHECK$>`                                                                                        Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$LASTSERVICESTATECHANGE$ <$LASTSERVICESTATECHANGE$>`                                                                            Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$LASTSERVICEOK$ <$LASTSERVICEOK$>`                                                                                              Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$LASTSERVICEWARNING$ <$LASTSERVICEWARNING$>`                                                                                    Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$LASTSERVICEUNKNOWN$ <$LASTSERVICEUNKNOWN$>`                                                                                    Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$LASTSERVICECRITICAL$ <$LASTSERVICECRITICAL$>`                                                                                  Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEOUTPUT$ <$SERVICEOUTPUT$>`                                                                                              Yes :ref:`02 <annexes/macros_list#note2>`                    Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$LONGSERVICEOUTPUT$ <$LONGSERVICEOUTPUT$>`                                                                                      Yes :ref:`02 <annexes/macros_list#note2>`                    Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEPERFDATA$ <$SERVICEPERFDATA$>`                                                                                          Yes :ref:`02 <annexes/macros_list#note2>`                    Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICECHECKCOMMAND$ <$SERVICECHECKCOMMAND$>`                                                                                  Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEACKAUTHOR$ <$SERVICEACKAUTHOR$>` :ref:`08 <annexes/macros_list#note8>`                                                  No                                                           Yes                                                          No                                                           No                                                           No                                                                                                                No                                                                                                             No                No
:ref:`$SERVICEACKAUTHORNAME$ <$SERVICEACKAUTHORNAME$>` :ref:`08 <annexes/macros_list#note8>`                                          No                                                           Yes                                                          No                                                           No                                                           No                                                                                                                No                                                                                                             No                No
:ref:`$SERVICEACKAUTHORALIAS$ <$SERVICEACKAUTHORALIAS$>` :ref:`08 <annexes/macros_list#note8>`                                        No                                                           Yes                                                          No                                                           No                                                           No                                                                                                                No                                                                                                             No                No
:ref:`$SERVICEACKCOMMENT$ <$SERVICEACKCOMMENT$>` :ref:`08 <annexes/macros_list#note8>`                                                No                                                           Yes                                                          No                                                           No                                                           No                                                                                                                No                                                                                                             No                No
:ref:`$SERVICEACTIONURL$ <$SERVICEACTIONURL$>`                                                                                        Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICENOTESURL$ <$SERVICENOTESURL$>`                                                                                          Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICENOTES$ <$SERVICENOTES$>`                                                                                                Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No
:ref:`$SERVICEBUSINESSIMPACT$ <$SERVICEBUSINESSIMPACT$>`                                                                              Yes                                                          Yes                                                          No                                                           No                                                           Yes                                                                                                               No                                                                                                             Yes               No

Macro Name                                                                                                                            Service Checks                                               Service Notifications                                        Host Checks                                                  Host Notifications                                           Service Event Handlers                                                                                            Host Event Handlers                                                                                            Service Perf Data Host Perf Data
Service Group Macros:
:ref:`$SERVICEGROUPALIAS$ <$SERVICEGROUPALIAS$>` :ref:`06 <annexes/macros_list#note6>`                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$SERVICEGROUPMEMBERS$ <$SERVICEGROUPMEMBERS$>` :ref:`06 <annexes/macros_list#note6>`                                            Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$SERVICEGROUPNOTES$ <$SERVICEGROUPNOTES$>` :ref:`06 <annexes/macros_list#note6>`                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$SERVICEGROUPNOTESURL$ <$SERVICEGROUPNOTESURL$>` :ref:`06 <annexes/macros_list#note6>`                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$SERVICEGROUPACTIONURL$ <$SERVICEGROUPACTIONURL$>` :ref:`06 <annexes/macros_list#note6>`                                        Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes

Macro Name                                                                                                                            Service Checks                                               Service Notifications                                        Host Checks                                                  Host Notifications                                           Service Event Handlers                                                                                            Host Event Handlers                                                                                            Service Perf Data Host Perf Data
Contact Macros:
:ref:`$CONTACTNAME$ <$CONTACTNAME$>`                                                                                                  No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$CONTACTALIAS$ <$CONTACTALIAS$>`                                                                                                No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$CONTACTEMAIL$ <$CONTACTEMAIL$>`                                                                                                No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$CONTACTPAGER$ <$CONTACTPAGER$>`                                                                                                No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$CONTACTADDRESSn$ <$CONTACTADDRESSn$>`                                                                                          No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No

Macro Name                                                                                                                            Service Checks                                               Service Notifications                                        Host Checks                                                  Host Notifications                                           Service Event Handlers                                                                                            Host Event Handlers                                                                                            Service Perf Data Host Perf Data
Contact Group Macros:
:ref:`$CONTACTGROUPALIAS$ <$CONTACTGROUPALIAS$>` :ref:`07 <annexes/macros_list#note7>`                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$CONTACTGROUPMEMBERS$ <$CONTACTGROUPMEMBERS$>` :ref:`07 <annexes/macros_list#note7>`                                            Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes

Macro Name                                                                                                                            Service Checks                                               Service Notifications                                        Host Checks                                                  Host Notifications                                           Service Event Handlers                                                                                            Host Event Handlers                                                                                            Service Perf Data Host Perf Data
Summary Macros:
:ref:`$TOTALHOSTSUP$ <$TOTALHOSTSUP$>` :ref:`10 <annexes/macros_list#note10>`                                                         Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALHOSTSDOWN$ <$TOTALHOSTSDOWN$>` :ref:`10 <annexes/macros_list#note10>`                                                     Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALHOSTSUNREACHABLE$ <$TOTALHOSTSUNREACHABLE$>` :ref:`10 <annexes/macros_list#note10>`                                       Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALHOSTSDOWNUNHANDLED$ <$TOTALHOSTSDOWNUNHANDLED$>` :ref:`10 <annexes/macros_list#note10>`                                   Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALHOSTSUNREACHABLEUNHANDLED$ <$TOTALHOSTSUNREACHABLEUNHANDLED$>` :ref:`10 <annexes/macros_list#note10>`                     Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALHOSTPROBLEMS$ <$TOTALHOSTPROBLEMS$>` :ref:`10 <annexes/macros_list#note10>`                                               Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALHOSTPROBLEMSUNHANDLED$ <$TOTALHOSTPROBLEMSUNHANDLED$>` :ref:`10 <annexes/macros_list#note10>`                             Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALSERVICESOK$ <$TOTALSERVICESOK$>` :ref:`10 <annexes/macros_list#note10>`                                                   Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALSERVICESWARNING$ <$TOTALSERVICESWARNING$>` :ref:`10 <annexes/macros_list#note10>`                                         Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALSERVICESCRITICAL$ <$TOTALSERVICESCRITICAL$>` :ref:`10 <annexes/macros_list#note10>`                                       Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALSERVICESUNKNOWN$ <$TOTALSERVICESUNKNOWN$>` :ref:`10 <annexes/macros_list#note10>`                                         Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALSERVICESWARNINGUNHANDLED$ <$TOTALSERVICESWARNINGUNHANDLED$>` :ref:`10 <annexes/macros_list#note10>`                       Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALSERVICESCRITICALUNHANDLED$ <$TOTALSERVICESCRITICALUNHANDLED$>` :ref:`10 <annexes/macros_list#note10>`                     Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALSERVICESUNKNOWNUNHANDLED$ <$TOTALSERVICESUNKNOWNUNHANDLED$>` :ref:`10 <annexes/macros_list#note10>`                       Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALSERVICEPROBLEMS$ <$TOTALSERVICEPROBLEMS$>` :ref:`10 <annexes/macros_list#note10>`                                         Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TOTALSERVICEPROBLEMSUNHANDLED$ <$TOTALSERVICEPROBLEMSUNHANDLED$>` :ref:`10 <annexes/macros_list#note10>`                       Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                          Yes :ref:`04 <annexes/macros_list#note4>`                    Yes                                                                                                               Yes                                                                                                            Yes               Yes

Macro Name                                                                                                                            Service Checks                                               Service Notifications                                        Host Checks                                                  Host Notifications                                           Service Event Handlers                                                                                            Host Event Handlers                                                                                            Service Perf Data Host Perf Data
Notification Macros:
:ref:`$NOTIFICATIONTYPE$ <$NOTIFICATIONTYPE$>`                                                                                        No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$NOTIFICATIONRECIPIENTS$ <$NOTIFICATIONRECIPIENTS$>`                                                                            No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$NOTIFICATIONISESCALATED$ <$NOTIFICATIONISESCALATED$>`                                                                          No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$NOTIFICATIONAUTHOR$ <$NOTIFICATIONAUTHOR$>`                                                                                    No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$NOTIFICATIONAUTHORNAME$ <$NOTIFICATIONAUTHORNAME$>`                                                                            No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$NOTIFICATIONAUTHORALIAS$ <$NOTIFICATIONAUTHORALIAS$>`                                                                          No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$NOTIFICATIONCOMMENT$ <$NOTIFICATIONCOMMENT$>`                                                                                  No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$NOTIFICATIONNUMBER$ <$NOTIFICATIONNUMBER$>`                                                                            No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$NOTIFICATIONID$ <$NOTIFICATIONID$>`                                                                                    No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$HOSTNOTIFICATIONNUMBER$ <$HOSTNOTIFICATIONNUMBER$>`                                                                            No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$HOSTNOTIFICATIONID$ <$HOSTNOTIFICATIONID$>`                                                                                    No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$SERVICENOTIFICATIONNUMBER$ <$SERVICENOTIFICATIONNUMBER$>`                                                                      No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No
:ref:`$SERVICENOTIFICATIONID$ <$SERVICENOTIFICATIONID$>`                                                                              No                                                           Yes                                                          No                                                           Yes                                                          No                                                                                                                No                                                                                                             No                No

Macro Name                                                                                                                            Service Checks                                               Service Notifications                                        Host Checks                                                  Host Notifications                                           Service Event Handlers                                                                                            Host Event Handlers                                                                                            Service Perf Data Host Perf Data
Date/Time Macros:
:ref:`$LONGDATETIME$ <$LONGDATETIME$>`                                                                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$SHORTDATETIME$ <$SHORTDATETIME$>`                                                                                              Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$DATE$ <$DATE$>`                                                                                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TIME$ <$TIME$>`                                                                                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TIMET$ <$TIMET$>`                                                                                                              Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$ISVALIDTIME:$ <$ISVALIDTIME$>`                                                                                                 Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$NEXTVALIDTIME:$ <$NEXTVALIDTIME$>`                                                                                             Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes

Macro Name                                                                                                                            Service Checks                                               Service Notifications                                        Host Checks                                                  Host Notifications                                           Service Event Handlers                                                                                            Host Event Handlers                                                                                            Service Perf Data Host Perf Data
File Macros:
:ref:`$ALIGNAK$ <$ALIGNAK$>`                                                                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$MAINCONFIGFILE$ <$MAINCONFIGFILE$>`                                                                                            Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$STATUSDATAFILE$ <$STATUSDATAFILE$>`                                                                                            Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$COMMENTDATAFILE$ <$COMMENTDATAFILE$>`                                                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes :ref:`05 <annexes/macros_list#note5>`
:ref:`$DOWNTIMEDATAFILE$ <$DOWNTIMEDATAFILE$>`                                                                                        Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$RETENTIONDATAFILE$ <$RETENTIONDATAFILE$>`                                                                                      Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$OBJECTCACHEFILE$ <$OBJECTCACHEFILE$>`                                                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TEMPFILE$ <$TEMPFILE$>`                                                                                                        Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$TEMPPATH$ <$TEMPPATH$>`                                                                                                        Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$LOGFILE$ <$LOGFILE$>`                                                                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$RESOURCEFILE$ <$RESOURCEFILE$>`                                                                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$COMMANDFILE$ <$COMMANDFILE$>`                                                                                                  Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$HOSTPERFDATAFILE$ <$HOSTPERFDATAFILE$>`                                                                                        Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$SERVICEPERFDATAFILE$ <$SERVICEPERFDATAFILE$>`                                                                                  Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes

Macro Name                                                                                                                            Service Checks                                               Service Notifications                                        Host Checks                                                  Host Notifications                                           Service Event Handlers                                                                                            Host Event Handlers                                                                                            Service Perf Data Host Perf Data
Misc Macros:
:ref:`$PROCESSSTARTTIME$ <$PROCESSSTARTTIME$>`                                                                                        Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$EVENTSTARTTIME$ <$EVENTSTARTTIME$>`                                                                                            Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$ADMINEMAIL$ <$ADMINEMAIL$>`                                                                                                    Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$ADMINPAGER$ <$ADMINPAGER$>`                                                                                                    Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$ARGn$ <$ARGn$>`                                                                                                                Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
:ref:`$USERn$ <$USERn$>`                                                                                                              Yes                                                          Yes                                                          Yes                                                          Yes                                                          Yes                                                                                                               Yes                                                                                                            Yes               Yes
===================================================================================================================================== ============================================================ ============================================================ ============================================================ ============================================================ ================================================================================================================= ============================================================================================================== ================= =================================================================


Macro Descriptions
==================


.. _annexes/macros_list#hostname:

Host Macros :ref:`03 <annexes/macros_list#note3>`
-------------------------------------------------

_`$HOSTNAME$`

Short name for the host (i.e. "biglinuxbox"). This value is taken from the host_name directive in the host definition.

_`$HOSTDISPLAYNAME$`

An alternate display name for the host. This value is taken from the display_name directive in the host definition.

_`$HOSTALIAS$`

Long name/description for the host. This value is taken from the alias directive in the host definition.

_`$HOSTADDRESS$`

Address of the host. This value is taken from the address directive in the host definition.

_`$HOSTSTATE$`

A string indicating the current state of the host ("UP", "DOWN", or "UNREACHABLE").

_`$HOSTSTATEID$`

A number that corresponds to the current state of the host: 0=UP, 1=DOWN, 2=UNREACHABLE.

_`$LASTHOSTSTATE$`

A string indicating the last state of the host ("UP", "DOWN", or "UNREACHABLE").

_`$LASTHOSTSTATEID$`

A number that corresponds to the last state of the host: 0=UP, 1=DOWN, 2=UNREACHABLE.

_`$HOSTSTATETYPE$`

A string indicating the :ref:`state type <monitoring_features/statetypes>` for the current host check ("HARD" or "SOFT").

_`$HOSTATTEMPT$`

The number of the current host check retry. For instance, if this is the second time that the host is being rechecked, this will be the number two. Current attempt number is really only useful when writing host event handlers for "soft" states that take a specific action based on the host retry number.

_`$MAXHOSTATTEMPTS$`

The max check attempts as defined for the current host. Useful when writing host event handlers for "soft" states that take a specific action based on the host retry number.

_`$HOSTEVENTID$`

A globally unique number associated with the host's current state. Every time a host (or service) experiences a state change, a global event ID number is incremented by one (1). If a host has experienced no state changes, this macro will be set to zero (0).

_`$LASTHOSTEVENTID$`

The previous (globally unique) event number that was given to the host.

_`$HOSTPROBLEMID$`

A globally unique number associated with the host's current problem state. Every time a host (or service) transitions from an UP or OK state to a problem state, a global problem ID number is incremented by one (1). This macro will be non-zero if the host is currently a non-UP state. State transitions between non-UP states (e.g. DOWN to UNREACHABLE) do not cause this problem id to increase. If the host is currently in an UP state, this macro will be set to zero (0). Combined with event handlers, this macro could be used to automatically open trouble tickets when hosts first enter a problem state.

_`$LASTHOSTPROBLEMID$`

The previous (globally unique) problem number that was given to the host. Combined with event handlers, this macro could be used for automatically closing trouble tickets, etc. when a host recovers to an UP state.

_`$HOSTLATENCY$`

A (floating point) number indicating the number of seconds that a scheduled host check lagged behind its scheduled check time. For instance, if a check was scheduled for 03:14:15 and it didn't get executed until 03:14:17, there would be a check latency of 2.0 seconds. On-demand host checks have a latency of zero seconds.

_`$HOSTEXECUTIONTIME$`

A (floating point) number indicating the number of seconds that the host check took to execute (i.e. the amount of time the check was executing).

_`$HOSTDURATION$`

A string indicating the amount of time that the host has spent in its current state. Format is "XXh YYm ZZs", indicating hours, minutes and seconds.

_`$HOSTDURATIONSEC$`

A number indicating the number of seconds that the host has spent in its current state.

_`$HOSTDOWNTIME$`

A number indicating the current "downtime depth" for the host. If this host is currently in a period of scheduled downtime, the value will be greater than zero. If the host is not currently in a period of downtime, this value will be zero.

_`$HOSTPERCENTCHANGE$`

A (floating point) number indicating the percent state change the host has undergone. Percent state change is used by the :ref:`flap detection <monitoring_features/flapping>` algorithm.

_`$HOSTGROUPNAME$`

The short name of the hostgroup that this host belongs to. This value is taken from the hostgroup_name directive in the hostgroup definition. If the host belongs to more than one hostgroup this macro will contain the name of just one of them.

_`$HOSTGROUPNAMES$`

A comma separated list of the short names of all the hostgroups that this host belongs to.

_`$LASTHOSTCHECK$`

This is a timestamp in time_t format (seconds since the UNIX epoch) indicating the time at which a check of the host was last performed.

_`$LASTHOSTSTATECHANGE$`

This is a timestamp in time_t format (seconds since the UNIX epoch) indicating the time the host last changed state.

_`$LASTHOSTUP$`

This is a timestamp in time_t format (seconds since the UNIX epoch) indicating the time at which the host was last detected as being in an UP state.

_`$LASTHOSTDOWN$`

This is a timestamp in time_t format (seconds since the UNIX epoch) indicating the time at which the host was last detected as being in a DOWN state.

_`$LASTHOSTUNREACHABLE$`

This is a timestamp in time_t format (seconds since the UNIX epoch) indicating the time at which the host waslast detected as being in an UNREACHABLE state.

_`$HOSTOUTPUT$`

The first line of text output from the last host check (i.e. "Ping OK").

_`$LONGHOSTOUTPUT$`

The full text output (aside from the first line) from the last host check.

_`$HOSTPERFDATA$`

This macro contains any performance data that may have been returned by the last host check.

_`$HOSTCHECKCOMMAND$`

This macro contains the name of the command (along with any arguments passed to it) used to perform the host check.

_`$HOSTACKAUTHOR$`  :ref:`08 <annexes/macros_list#note8>`

A string containing the name of the user who acknowledged the host problem. This macro is only valid in notifications where the $NOTIFICATIONTYPE$ macro is set to "ACKNOWLEDGEMENT".

_`$HOSTACKAUTHORNAME$`  :ref:`08 <annexes/macros_list#note8>`

A string containing the short name of the contact (if applicable) who acknowledged the host problem. This macro is only valid in notifications where the $NOTIFICATIONTYPE$ macro is set to "ACKNOWLEDGEMENT".

_`$HOSTACKAUTHORALIAS$`  :ref:`08 <annexes/macros_list#note8>`

A string containing the alias of the contact (if applicable) who acknowledged the host problem. This macro is only valid in notifications where the $NOTIFICATIONTYPE$ macro is set to "ACKNOWLEDGEMENT".

_`$HOSTACKCOMMENT$`  :ref:`08 <annexes/macros_list#note8>`

A string containing the acknowledgement comment that was entered by the user who acknowledged the host problem. This macro is only valid in notifications where the $NOTIFICATIONTYPE$ macro is set to "ACKNOWLEDGEMENT".

_`$HOSTACTIONURL$`

Action URL for the host. This macro may contain other macros (e.g. $HOSTNAME$), which can be useful when you want to pass the host name to a web page.

_`$HOSTNOTESURL$`

Notes URL for the host. This macro may contain other macros (e.g. $HOSTNAME$), which can be useful when you want to pass the host name to a web page.

_`$HOSTNOTES$`

Notes for the host. This macro may contain other macros (e.g. $HOSTNAME$), which can be useful when you want to host-specific status information, etc. in the description.

_`$HOSTBUSINESSIMPACT$`

A number indicating the business impact for the host.

_`$TOTALHOSTSERVICES$`

The total number of services associated with the host.

_`$TOTALHOSTSERVICESOK$`

The total number of services associated with the host that are in an OK state.

_`$TOTALHOSTSERVICESWARNING$`

The total number of services associated with the host that are in a WARNING state.

_`$TOTALHOSTSERVICESUNKNOWN$`

The total number of services associated with the host that are in an UNKNOWN state.

_`$TOTALHOSTSERVICESUNREACHABLE$`

The total number of services associated with the host that are in an UNREACHABLE state.

_`$TOTALHOSTSERVICESCRITICAL$`
The total number of services associated with the host that are in a CRITICAL state.


Host Group Macros :ref:`05 <annexes/macros_list#note5>`
-------------------------------------------------------

_`$HOSTGROUPALIAS$`  :ref:`05 <annexes/macros_list#note5>`

The long name / alias of either 1) the hostgroup name passed as an on_demand macro argument or 2) the primary hostgroup associated with the current host (if not used in the context of an on_demand macro). This value is taken from the alias directive in the hostgroup definition.

_`$HOSTGROUPMEMBERS$`  :ref:`05 <annexes/macros_list#note5>`

A comma-separated list of all hosts that belong to either 1) the hostgroup name passed as an on-demand macro argument or 2) the primary hostgroup associated with the current host (if not used in the context of an on-demand macro).

_`$HOSTGROUPNOTES$`  :ref:`05 <annexes/macros_list#note5>`

The notes associated with either 1) the hostgroup name passed as an on_demand macro argument or 2) the primary hostgroup associated with the current host (if not used in the context of an on_demand macro). This value is taken from the notes directive in the hostgroup definition.

_`$HOSTGROUPNOTESURL$`  :ref:`05 <annexes/macros_list#note5>`

The notes URL associated with either 1) the hostgroup name passed as an on_demand macro argument or 2) the primary hostgroup associated with the current host (if not used in the context of an on_demand macro). This value is taken from the notes_url directive in the hostgroup definition.

_`$HOSTGROUPACTIONURL$`  :ref:`05 <annexes/macros_list#note5>`

The action URL associated with either 1) the hostgroup name passed as an on_demand macro argument or 2) the primary hostgroup associated with the current host (if not used in the context of an on_demand macro). This value is taken from the action_url directive in the hostgroup definition.


.. _annexes/macros_list#longserviceoutput:
.. _annexes/macros_list#serviceperfdata:

Service Macros
--------------

$SERVICEDESC$`

The long name/description of the service (i.e. "Main Website"). This value is taken from the description directive of the service definition.


_`$SERVICEDISPLAYNAME$`

An alternate display name for the service. This value is taken from the display_name directive in the service definition.


_`$SERVICESTATE$`

A string indicating the current state of the service ("OK", "WARNING", "UNKNOWN", or "CRITICAL").


_`$SERVICESTATEID$`

A number that corresponds to the current state of the service: 0=OK, 1=WARNING, 2=CRITICAL, 3=UNKNOWN.


_`$LASTSERVICESTATE$`

A string indicating the last state of the service ("OK", "WARNING", "UNKNOWN", or "CRITICAL").


_`$LASTSERVICESTATEID$`

A number that corresponds to the last state of the service: 0=OK, 1=WARNING, 2=CRITICAL, 3=UNKNOWN.

_`$SERVICESTATETYPE$`

A string indicating the state type of the current service check ("HARD" or "SOFT").

_`$SERVICEATTEMPT$`

The number of the current service check retry. For instance, if this is the second time that the service is being rechecked, this will be the number two. Current attempt number is really only useful when writing service event handlers for "soft" states that take a specific action based on the service retry number.

_`$MAXSERVICEATTEMPTS$`

The max check attempts as defined for the current service. Useful when writing host event handlers for "soft" states that take a specific action based on the service retry number.

_`$SERVICEISVOLATILE$`

Indicates whether the service is marked as being volatile or not: 0 = not volatile, 1 = volatile.

_`$SERVICEEVENTID$`

A globally unique number associated with the service's current state. Every time a a service (or host) experiences a state change, a global event ID number is incremented by one (1). If a service has experienced no state changes, this macro will be set to zero (0).

_`$LASTSERVICEEVENTID$`

The previous (globally unique) event number that given to the service.

_`$SERVICEPROBLEMID$`

A globally unique number associated with the service's current problem state. Every time a service (or host) transitions from an OK or UP state to a problem state, a global problem ID number is incremented by one (1). This macro will be non-zero if the service is currently a non-OK state. State transitions between non-OK states (e.g. WARNING to CRITICAL) do not cause this problem id to increase. If the service is currently in an OK state, this macro will be set to zero (0). Combined with event handlers, this macro could be used to automatically open trouble tickets when services first enter a problem state.

_`$LASTSERVICEPROBLEMID$`

The previous (globally unique) problem number that was given to the service. Combined with event handlers, this macro could be used for automatically closing trouble tickets, etc. when a service recovers to an OK state.

_`$SERVICELATENCY$`

A (floating point) number indicating the number of seconds that a scheduled service check lagged behind its scheduled check time. For instance, if a check was scheduled for 03:14:15 and it didn't get executed until 03:14:17, there would be a check latency of 2.0 seconds.

_`$SERVICEEXECUTIONTIME$`

A (floating point) number indicating the number of seconds that the service check took to execute (i.e. the amount of time the check was executing).

_`$SERVICEDURATION$`

A string indicating the amount of time that the service has spent in its current state. Format is "XXh YYm ZZs", indicating hours, minutes and seconds.

_`$SERVICEDURATIONSEC$`

A number indicating the number of seconds that the service has spent in its current state.

_`$SERVICEDOWNTIME$`

A number indicating the current "downtime depth" for the service. If this service is currently in a period of scheduled downtime, the value will be greater than zero. If the service is not currently in a period of downtime, this value will be zero.

_`$SERVICEPERCENTCHANGE$`

A (floating point) number indicating the percent state change the service has undergone. Percent state change is used by the :ref:`flap detection <monitoring_features/flapping>` algorithm.

_`$SERVICEGROUPNAME$`

The short name of the servicegroup that this service belongs to. This value is taken from the servicegroup_name directive in the servicegroup definition. If the service belongs to more than one servicegroup this macro will contain the name of just one of them.

_`$SERVICEGROUPNAMES$`

A comma separated list of the short names of all the servicegroups that this service belongs to.

_`$LASTSERVICECHECK$`

This is a timestamp in time_t format (seconds since the UNIX epoch) indicating the time at which a check of the service was last performed.

_`$LASTSERVICESTATECHANGE$`

This is a timestamp in time_t format (seconds since the UNIX epoch) indicating the time the service last changed state.

_`$LASTSERVICEOK$`

This is a timestamp in time_t format (seconds since the UNIX epoch) indicating the time at which the service was last detected as being in an OK state.

_`$LASTSERVICEWARNING$`

This is a timestamp in time_t format (seconds since the UNIX epoch) indicating the time at which the service was last detected as being in a WARNING state.

_`$LASTSERVICEUNKNOWN$`

This is a timestamp in time_t format (seconds since the UNIX epoch) indicating the time at which the service was last detected as being in an UNKNOWN state.

_`$LASTSERVICEUNREACHABLE$`

This is a timestamp in time_t format (seconds since the UNIX epoch) indicating the time at which the service was last detected as being in an UNREACHABLE state.

_`$LASTSERVICECRITICAL$`

This is a timestamp in time_t format (seconds since the UNIX epoch) indicating the time at which the service was last detected as being in a CRITICAL state.

_`$SERVICEOUTPUT$`

The first line of text output from the last service check (i.e. "Ping OK").

_`$LONGSERVICEOUTPUT$`

The full text output (aside from the first line) from the last service check.

_`$SERVICEPERFDATA$`

This macro contains any performance data that may have been returned by the last service check.

_`$SERVICECHECKCOMMAND$`

This macro contains the name of the command (along with any arguments passed to it) used to perform the service check.

_`$SERVICEACKAUTHOR$` :ref:`08 <annexes/macros_list#note8>`

A string containing the name of the user who acknowledged the service problem. This macro is only valid in notifications where the $NOTIFICATIONTYPE$ macro is set to "ACKNOWLEDGEMENT".

_`$SERVICEACKAUTHORNAME$` :ref:`08 <annexes/macros_list#note8>`

A string containing the short name of the contact (if applicable) who acknowledged the service problem. This macro is only valid in notifications where the $NOTIFICATIONTYPE$ macro is set to "ACKNOWLEDGEMENT".

_`$SERVICEACKAUTHORALIAS$` :ref:`08 <annexes/macros_list#note8>`

A string containing the alias of the contact (if applicable) who acknowledged the service problem. This macro is only valid in notifications where the $NOTIFICATIONTYPE$ macro is set to "ACKNOWLEDGEMENT".

_`$SERVICEACKCOMMENT$` :ref:`08 <annexes/macros_list#note8>`

A string containing the acknowledgement comment that was entered by the user who acknowledged the service problem. This macro is only valid in notifications where the $NOTIFICATIONTYPE$ macro is set to "ACKNOWLEDGEMENT".

_`$SERVICEACTIONURL$`

Action URL for the service. This macro may contain other macros (e.g. $HOSTNAME$ or $SERVICEDESC$), which can be useful when you want to pass the service name to a web page.

_`$SERVICENOTESURL$`

Notes URL for the service. This macro may contain other macros (e.g. $HOSTNAME$ or $SERVICEDESC$), which can be useful when you want to pass the service name to a web page.

_`$SERVICENOTES$`

Notes for the service. This macro may contain other macros (e.g. $HOSTNAME$ or $SERVICESTATE$), which can be useful when you want to service-specific status information, etc. in the description

_`$SERVICEBUSINESSIMPACT$`

A number indicating the business impact for the service.


Service Group Macros :ref:`06 <annexes/macros_list#note6>`
-----------------------------------------------------------

_`$SERVICEGROUPALIAS$` :ref:`06 <annexes/macros_list#note6>`

The long name / alias of either 1) the servicegroup name passed as an on_demand macro argument or 2) the primary servicegroup associated with the current service (if not used in the context of an on_demand macro). This value is taken from the alias directive in the servicegroup definition.

_`$SERVICEGROUPMEMBERS$` :ref:`06 <annexes/macros_list#note6>`

A comma-separated list of all services that belong to either 1) the servicegroup name passed as an on-demand macro argument or 2) the primary servicegroup associated with the current service (if not used in the context of an on-demand macro).

_`$SERVICEGROUPNOTES$` :ref:`06 <annexes/macros_list#note6>`

The notes associated with either 1) the servicegroup name passed as an on_demand macro argument or 2) the primary servicegroup associated with the current service (if not used in the context of an on_demand macro). This value is taken from the notes directive in the servicegroup definition.

_`$SERVICEGROUPNOTESURL$` :ref:`06 <annexes/macros_list#note6>`

The notes URL associated with either 1) the servicegroup name passed as an on_demand macro argument or 2) the primary servicegroup associated with the current service (if not used in the context of an on_demand macro). This value is taken from the notes_url directive in the servicegroup definition.

_`$SERVICEGROUPACTIONURL$` :ref:`06 <annexes/macros_list#note6>`

The action URL associated with either 1) the servicegroup name passed as an on_demand macro argument or 2) the primary servicegroup associated with the current service (if not used in the context of an on_demand macro). This value is taken from the action_url directive in the servicegroup definition.


Contact Macros
--------------

_`$CONTACTNAME$`

Short name for the contact (i.e. "jdoe") that is being notified of a host or service problem. This value is taken from the contact_name directive in the contact definition.

_`$CONTACTALIAS$`

Long name/description for the contact (i.e. "John Doe") being notified. This value is taken from the alias directive in the contact definition.

_`$CONTACTEMAIL$`

Email address of the contact being notified. This value is taken from the email directive in the contact definition.

_`$CONTACTPAGER$`

Pager number/address of the contact being notified. This value is taken from the pager directive in the contact definition.

_`$CONTACTADDRESSn$`

Address of the contact being notified. Each contact can have six different addresses (in addition to email address and pager number). The macros for these addresses are $CONTACTADDRESS1$ - $CONTACTADDRESS6$. This value is taken from the addressx directive in the contact definition.

_`$CONTACTGROUPNAME$`

The short name of the contactgroup that this contact is a member of. This value is taken from the contactgroup_name directive in the contactgroup definition. If the contact belongs to more than one contactgroup this macro will contain the name of just one of them.

_`$CONTACTGROUPNAMES$`

A comma separated list of the short names of all the contactgroups that this contact is a member of.


Contact Group Macros :ref:`05 <annexes/macros_list#note5>`
-----------------------------------------------------------

_`$CONTACTGROUPALIAS$`  :ref:`07 <annexes/macros_list#note7>`

The long name / alias of either 1) the contactgroup name passed as an on_demand macro argument or 2) the primary contactgroup associated with the current contact (if not used in the context of an on_demand macro). This value is taken from the alias directive in the contactgroup definition.

_`$CONTACTGROUPMEMBERS$`  :ref:`07 <annexes/macros_list#note7>`

A comma-separated list of all contacts that belong to either 1) the contactgroup name passed as an on-demand macro argument or 2) the primary contactgroup associated with the current contact (if not used in the context of an on-demand macro).


Summary Macros
--------------

_`$TOTALHOSTSUP$`

This macro reflects the total number of hosts that are currently in an UP state.

_`$TOTALHOSTSDOWN$`

This macro reflects the total number of hosts that are currently in a DOWN state.

_`$TOTALHOSTSUNREACHABLE$`

This macro reflects the total number of hosts that are currently in an UNREACHABLE state.

_`$TOTALHOSTSDOWNUNHANDLED$`

This macro reflects the total number of hosts that are currently in a DOWN state that are not currently being "handled". Unhandled host problems are those that are not acknowledged, are not currently in scheduled downtime, and for which checks are currently enabled.

_`$TOTALHOSTSUNREACHABLEUNHANDLED$`

This macro reflects the total number of hosts that are currently in an UNREACHABLE state that are not currently being "handled". Unhandled host problems are those that are not acknowledged, are not currently in scheduled downtime, and for which checks are currently enabled.

_`$TOTALHOSTPROBLEMS$`

This macro reflects the total number of hosts that are currently either in a DOWN or an UNREACHABLE state.

_`$TOTALHOSTPROBLEMSUNHANDLED$`

This macro reflects the total number of hosts that are currently either in a DOWN or an UNREACHABLE state that are not currently being "handled". Unhandled host problems are those that are not acknowledged, are not currently in scheduled downtime, and for which checks are currently enabled.

_`$TOTALSERVICESOK$`

This macro reflects the total number of services that are currently in an OK state.

_`$TOTALSERVICESWARNING$`

This macro reflects the total number of services that are currently in a WARNING state.

_`$TOTALSERVICESCRITICAL$`

This macro reflects the total number of services that are currently in a CRITICAL state.

_`$TOTALSERVICESUNKNOWN$`

This macro reflects the total number of services that are currently in an UNKNOWN state.

_`$TOTALSERVICESUNREACHABLE$`

This macro reflects the total number of services that are currently in an UNREACHABLE state.

_`$TOTALSERVICESWARNINGUNHANDLED$`


This macro reflects the total number of services that are currently in a WARNING state that are not currently being "handled". Unhandled services problems are those that are not acknowledged, are not currently in scheduled downtime, and for which checks are currently enabled.

_`$TOTALSERVICESCRITICALUNHANDLED$`

This macro reflects the total number of services that are currently in a CRITICAL state that are not currently being "handled". Unhandled services problems are those that are not acknowledged, are not currently in scheduled downtime, and for which checks are currently enabled.

_`$TOTALSERVICESUNKNOWNUNHANDLED$`

This macro reflects the total number of services that are currently in an UNKNOWN state that are not currently being "handled". Unhandled services problems are those that are not acknowledged, are not currently in scheduled downtime, and for which checks are currently enabled.

_`$TOTALSERVICEPROBLEMS$`

This macro reflects the total number of services that are currently either in a WARNING, CRITICAL, or UNKNOWN state.

_`$TOTALSERVICEPROBLEMSUNHANDLED$`

This macro reflects the total number of services that are currently either in a WARNING, CRITICAL, or UNKNOWN state that are not currently being "handled". Unhandled services problems are those that are not acknowledged, are not currently in scheduled downtime, and for which checks are currently enabled.


Notification Macros
-------------------

_`$NOTIFICATIONTYPE$`

A string identifying the type of notification that is being sent ("PROBLEM", "RECOVERY", "ACKNOWLEDGEMENT", "FLAPPINGSTART", "FLAPPINGSTOP", "FLAPPINGDISABLED", "DOWNTIMESTART", "DOWNTIMEEND", or "DOWNTIMECANCELLED").

_`$NOTIFICATIONRECIPIENTS$`

A comma-separated list of the short names of all contacts that are being notified about the host or service.

_`$NOTIFICATIONISESCALATED$`

An integer indicating whether this was sent to normal contacts for the host or service or if it was escalated. 0 = Normal (non-escalated) notification , 1 = Escalated notification.

_`$NOTIFICATIONAUTHOR$`

A string containing the name of the user who authored the notification. If the $NOTIFICATIONTYPE$ macro is set to "DOWNTIMESTART" or "DOWNTIMEEND", this will be the name of the user who scheduled downtime for the host or service. If the $NOTIFICATIONTYPE$ macro is "ACKNOWLEDGEMENT", this will be the name of the user who acknowledged the host or service problem. If the $NOTIFICATIONTYPE$ macro is "CUSTOM", this will be name of the user who initiated the custom host or service notification.

_`$NOTIFICATIONAUTHORNAME$`

A string containing the short name of the contact (if applicable) specified in the $NOTIFICATIONAUTHOR$ macro.

_`$NOTIFICATIONAUTHORALIAS$`

A string containing the alias of the contact (if applicable) specified in the $NOTIFICATIONAUTHOR$ macro.

_`$NOTIFICATIONCOMMENT$`

A string containing the comment that was entered by the notification author.

If the $NOTIFICATIONTYPE$ macro is set to "DOWNTIMESTART" or "DOWNTIMEEND", this will be the comment entered by the user who scheduled downtime for the host or service.

If the $NOTIFICATIONTYPE$ macro is "ACKNOWLEDGEMENT", this will be the comment entered by the user who acknowledged the host or service problem.

If the $NOTIFICATIONTYPE$ macro is "CUSTOM", this will be comment entered by the user who initiated the custom host or service notification.

_`$NOTIFICATIONNUMBER$`

The current notification number for the host/service. You can use this macro in place of the $HOSTNOTIFICATIONNUMBER$ and $SERVICENOTIFICATIONNUMBER$.

_`$NOTIFICATIONID$`

A unique number identifying a notification. You can use this macro in place of the $HOSTNOTIFICATIONID$ and $SERVICENOTIFICATIONID$.

_`$HOSTNOTIFICATIONNUMBER$`

The current notification number for the host. The notification number increases by one (1) each time a new notification is sent out for the host (except for acknowledgements).

The notification number is reset to 0 when the host recovers (after the recovery notification has gone out). Acknowledgements do not cause the notification number to increase, nor do notifications dealing with flap detection or scheduled downtime.

_`$HOSTNOTIFICATIONID$`

A unique number identifying a host notification.

Notification ID numbers are unique across both hosts and service notifications, so you could potentially use this unique number as a primary key in a notification database. Notification ID numbers should remain unique across restarts of the Alignak process, so long as you have state retention enabled.

The notification ID number is incremented by one (1) each time a new host notification is sent out, and regardless of how many contacts are notified.

_`$SERVICENOTIFICATIONNUMBER$`

The current notification number for the service. The notification number increases by one (1) each time a new notification is sent out for the service (except for acknowledgements).

The notification number is reset to 0 when the service recovers (after the recovery notification has gone out). Acknowledgements do not cause the notification number to increase, nor do notifications dealing with flap detection or scheduled downtime.

_`$SERVICENOTIFICATIONID$`

A unique number identifying a service notification.

Notification ID numbers are unique across both hosts and service notifications, so you could potentially use this unique number as a primary key in a notification database. Notification ID numbers should remain unique across restarts of the Alignak process, so long as you have state retention enabled.

The notification ID number is incremented by one (1) each time a new service notification is sent out, and regardless of how many contacts are notified.


Date/Time Macros
----------------

_`$LONGDATETIME$`

Current date/time stamp (i.e. Fri Oct 13 00:30:28 CDT 2000).

_`$SHORTDATETIME$`

Current date/time stamp (i.e. 10-13-2000 00:30:28).

_`$DATE$`

Date stamp (i.e. 10-13-2000).

_`$TIME$`

Current time stamp (i.e. 00:30:28).
_`$TIMET$`

Current time stamp in time_t format (seconds since the UNIX epoch).

_`$ISVALIDTIME$`  :ref:`09 <annexes/macros_list#note9>`

This is a special on_demand macro that returns a 1 or 0 depending on whether or not a particular time is valid within a specified timeperiod. There are two ways of using this macro:     _ $ISVALIDTIME:24x7$ will be set to "1" if the current time is valid within the "24x7" timeperiod. If not, it will be set to "0".   _ $ISVALIDTIME:24x7:timestamp$ will be set to "1" if the time specified by the "timestamp" argument (which must be in time_t format) is valid within the "24x7" timeperiod. If not, it will be set to "0".

_`$NEXTVALIDTIME$`  :ref:`09 <annexes/macros_list#note9>`

This is a special on_demand macro that returns the next valid time (in time_t format) for a specified timeperiod. There are two ways of using this macro:     _ $NEXTVALIDTIME:24x7$ will return the next valid time _ from and including the current time _ in the "24x7" timeperiod.   _ $NEXTVALIDTIME:24x7:timestamp$ will return the next valid time - from and including the time specified by the "timestamp" argument (which must be specified in time_t format) - in the "24x7" timeperiod.If a next valid time cannot be found in the specified timeperiod, the macro will be set to "0".


File Macros
-----------
**Note** this list needs to be updated!
================================================================ ======================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================
_`$ALIGNAK$`                                                     The name defined for the Alignak instance in the configuration file.
_`$MAINCONFIGFILE$`                                              The location and name of the main configuration file.
_`$STATUSDATAFILE$`                                              Not available
_`$COMMENTDATAFILE$`                                             Not available
_`$DOWNTIMEDATAFILE$`                                            Not available
_`$RETENTIONDATAFILE$`                                           Not available
_`$OBJECTCACHEFILE$`                                             Not available
_`$TEMPFILE$`                                                    Not available
_`$TEMPPATH$`                                                    Not available
_`$LOGFILE$`                                                     Not available
_`$RESOURCEFILE$`                                                Not available
_`$COMMANDFILE$`                                                 Not available
_`$HOSTPERFDATAFILE$`                                            Not available
_`$SERVICEPERFDATAFILE$`                                         Not available
================================================================ ======================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================


.. _annexes/macros_list#usern:
.. _annexes/macros_list#argn:

Misc Macros
-----------
**Note** this list needs to be updated!

_`$PROCESSSTARTTIME$`

Time stamp in time_t format (seconds since the UNIX epoch) indicating when the Alignak process was last (re)started. You can determine the number of seconds that Alignak has been running (since it was last restarted) by subtracting $PROCESSSTARTTIME$ from :ref:`$TIMET$ <$TIMET$>`.

_`$EVENTSTARTTIME$`

Time stamp in time_t format (seconds since the UNIX epoch) indicating when the Alignak process starting process events (checks, etc.). You can determine the number of seconds that it took for Alignak to startup by subtracting $PROCESSSTARTTIME$ from $EVENTSTARTTIME$.

_`$ADMINEMAIL$` (unused)

Not available

_`$ADMINPAGER$` (unused)
Not available

_`$ARGn$`

The nth argument passed to the command (notification, event handler, service check, etc.). Alignak supports up to 32 argument macros ($ARG1$ through $ARG32$).

_`$USERn$`

The nth user-definable macro. Alignak supports up to 32 user macros ($USER1$ through $USER32$).


Notes
=====

.. _annexes/macros_list#note1:

  * **01** These macros are not valid for the host they are associated with when that host is being checked (i.e. they make no sense, as they haven't been determined yet).

.. _annexes/macros_list#note2:

  * **02** These macros are not valid for the service they are associated with when that service is being checked (i.e. they make no sense, as they haven't been determined yet).

.. _annexes/macros_list#note3:

  * **03** When host macros are used in service-related commands (i.e. service notifications, event handlers, etc) they refer to the host that the service is associated with.

.. _annexes/macros_list#note4:

  * **04** When host and service summary macros are used in notification commands, the totals are filtered to reflect only those hosts and services for which the contact is authorized (i.e. hosts and services they are configured to receive notifications for).

.. _annexes/macros_list#note5:

  * **05** These macros are normally associated with the first/primary hostgroup associated with the current host. They could therefore be considered host macros in many cases. However, these macros are not available as on-demand host macros. Instead, they can be used as on-demand hostgroup macros when you pass the name of a hostgroup to the macro. For example: $HOSTGROUPMEMBERS:hg1$ would return a comma-delimited list of all (host) members of the hostgroup hg1.

.. _annexes/macros_list#note6:

  * **06** These macros are normally associated with the first/primary servicegroup associated with the current service. They could therefore be considered service macros in many cases. However, these macros are not available as on-demand service macros. Instead, they can be used as on-demand servicegroup macros when you pass the name of a servicegroup to the macro. For example: $SERVICEGROUPMEMBERS:sg1$ would return a comma-delimited list of all (service) members of the servicegroup sg1.

.. _annexes/macros_list#note7:

  * **07** These macros are normally associated with the first/primary contactgroup associated with the current contact. They could therefore be considered contact macros in many cases. However, these macros are not available as on-demand contact macros. Instead, they can be used as on-demand contactgroup macros when you pass the name of a contactgroup to the macro. For example: $CONTACTGROUPMEMBERS:cg1$ would return a comma-delimited list of all (contact) members of the contactgroup cg1.

.. _annexes/macros_list#note8:

  * **08** These acknowledgement macros are deprecated. Use the more generic $NOTIFICATIONAUTHOR$, $NOTIFICATIONAUTHORNAME$, $NOTIFICATIONAUTHORALIAS$ or $NOTIFICATIONAUTHORCOMMENT$ macros instead.

.. _annexes/macros_list#note9:

  * **09** These macro are only available as on-demand macros - e.g. you must supply an additional argument with them in order to use them. These macros are not available as environment variables.

.. _annexes/macros_list#note10:

  * **10** Summary macros are not available as environment variables if the :ref:`use_large_installation_tweaks <configuration/core>` option is enabled, as they are quite CPU-intensive to calculate.

