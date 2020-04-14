# GetUniqueIDofPC
get unique id, CPU ID, Hard disk ID, Motherboard ID, UUID


        /// <summary>
        /// Get Motherboard Serial Number
        /// </summary>
        /// <returns></returns>
        public static string getMotherboardSN()
        {
            ManagementObjectSearcher mos = new ManagementObjectSearcher("SELECT * FROM Win32_BaseBoard");
            ManagementObjectCollection moc = mos.Get();
            string serial = "";
            foreach (ManagementObject mo in moc)
            {
                serial = (string)mo["SerialNumber"];
            }
            return serial; 
        }

        /// <summary>
        /// Get CPU ID
        /// </summary>
        /// <returns></returns>
        public static string getProcessID()
        {
            ManagementObjectCollection mbsList = null;
            ManagementObjectSearcher mbs = new ManagementObjectSearcher("Select * From Win32_processor");
            mbsList = mbs.Get();
            string id = "";
            foreach (ManagementObject mo in mbsList)
            {
                //CPU ID
                id = mo["ProcessorID"].ToString(); 
            }
            return id; 
        }

        /// <summary>
        /// Get hard disk ID
        /// </summary>
        /// <param name="partitiondrive">Partition Drive,like "C"</param>
        /// <returns></returns>
        public static string getHardDiskID(string partitiondrive)
        {
            ManagementObject dsk = new ManagementObject(@"win32_logicaldisk.deviceid=""" + partitiondrive + @":""");
            dsk.Get();
            return dsk["VolumeSerialNumber"].ToString();
        }

        /// <summary>
        /// Get UUID
        /// </summary>
        /// <returns></returns>
        public static string getUUID()
        {
            string ComputerName = "localhost";
            ManagementScope Scope;
            Scope = new ManagementScope(String.Format("\\\\{0}\\root\\CIMV2", ComputerName), null);
            Scope.Connect();
            ObjectQuery Query = new ObjectQuery("SELECT UUID FROM Win32_ComputerSystemProduct");
            ManagementObjectSearcher Searcher = new ManagementObjectSearcher(Scope, Query);
            string UUID = String.Empty;
            foreach (ManagementObject WmiObject in Searcher.Get())
            {
                UUID = WmiObject["UUID"].ToString();
            }
            return UUID;
        } 
