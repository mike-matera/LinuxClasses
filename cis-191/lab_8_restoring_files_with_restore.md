IntroductionIn this lab you will use the restore command to restore files from a backup. The backup files have been previously made and are listed below.
  * [home_backup.full.0.dump](http://ea994bd4-a-9f1c7e1e-s-sites.googlegroups.com/a/lifealgorithmic.com/cabrillo-home/home/cis-191/lab-8-restoring-files-with-restore/home_backup.full.0.dump?attachauth=ANoY7cp95PlUusBJcV4lEn6RBqFtYhDop55wqKTfPVaVtAOyog08hxaUY38rxDOp_I1Ht1_WxpmjkX6MnGSh4lPh22DvORkldb4oDIg4uVdlKdhknMzigNnR_6EkiyO5HFcZfGPIyzwLXHhJ5nv3EM_hMMsfsRWZqqjPSgJ_MrTLfkzW34hbDyEva4NEgK2vWahiRRh_EphvONJM2ce5BztiY9gU08Yb7pSHTbZbIKCZvY7dpla67lR8580XtsCuD7P2T_3HxPwBtf5fEgmHhe7uXzcFfhvrH1UBo9anye929py82ShyXFk%3D&attredirects=0&d=1) - A full backup
  * [home_backup.incr.5.dump](http://ea994bd4-a-9f1c7e1e-s-sites.googlegroups.com/a/lifealgorithmic.com/cabrillo-home/home/cis-191/lab-8-restoring-files-with-restore/home_backup.incr.5.dump?attachauth=ANoY7cr2__H2UGjJJetakMTEov5bR92DaHPdOH5CvzzBrdGTYwDK_NCnErLJ4wsp6wxQmaKK1jiJV9e9dtw5l4zpAjMfbHaoe1fluV5Qtw39Cu3L7Q0Ds5VqzLRlf_8QYaH0cswHw6QCthpkmF_EzXlgPkyUVjg5X4DyCRW7h2aYCmjW_byR4n3oInvxyXHoop4AL9ruNqyYGqMmPoFENkCd48Q6ZydhDaFKCJFkoi9v0wkac5OMnlj5N6xa8X74UaTqaJg4Dr8vutsV9x99N_0zkCdG_YKeutqVeHkj4M_BLgrMiVfwVB4%3D&attredirects=0&d=1) - An incremental backup (from the full backup above)
  * [home_backup.diff.9.dump](http://ea994bd4-a-9f1c7e1e-s-sites.googlegroups.com/a/lifealgorithmic.com/cabrillo-home/home/cis-191/lab-8-restoring-files-with-restore/home_backup.diff.9.dump?attachauth=ANoY7cruP4gi0c0RYjL4_XdcJFnTE_pMMRJdha4Tt429SfrK3QDqrXarrO7l8nU_f0DDceYc_thjePWPZeO5XYTMhkkpfBVWiiNLyehosi88IBgl0eM_4L8ewsrLcuaRF_8MsCsiNyWGsnmzDzGybzuwj_AmLUannANtKGd7opUEoX0qXfSWUWVkBVB40jXoe54i5VG_xDk5AOmAIY3r_mM1Jnq0UNuGcSE2B2iqFQ5liEBIGD48SyFklXnLdCduOHbIn2V_LmWI1wq_giuuyC_YT79vDC5aztmU1BMRq8Jk_L3nv2ve8GY%3D&attredirects=0&d=1) - A differential backup (from the incremental backup above)

Part 1: Restore the Complete set of FilesIn this part you will restore the backups into a directory that you chose. When you restore into a directory you will not need root permissions, restore will complain but the complaints are safe to ignore. Remember to restore to the most recent available version of all files. When you are done create a TAR archive of your directory.
Part 2: Restore a Deleted FileThe user 'user2' accidentally deleted a file. But he's not exactly sure when. The deleted file is:
/home/user2/fileC
You must now look through the backups and find the last time that fileC existed. When you find it extract it to the same directory you used for Part 1 above. When you've found the file make a TAR archive of the directory again with fileC restored.
Turn InPlease submit the following files
  * part1.tar - A TAR archive that's the result of the full restore in Part 1
  * part2.tar - A TAR archive that's the same as part1.tar except that it contains fileC

Grading
  * 10 points for a correct part1.tar
  * 10 points for a correct part2.tar

