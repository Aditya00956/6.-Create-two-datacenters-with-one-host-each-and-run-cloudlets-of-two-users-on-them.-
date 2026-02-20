 # Experiment 6  
## Create two datacenters with one host each and run cloudlets of two users on them

---

## Overview
This experiment demonstrates a CloudSim simulation where two users submit cloudlets that execute across two different datacenters. Each datacenter contains one host, and tasks are scheduled using brokers representing users. The experiment shows distributed task execution and resource allocation in a cloud environment.

---

## Aim
To create two datacenters with one host each and execute cloudlets of two different users on them.

---

## Objectives
- Simulate multiple datacenters
- Simulate multiple users
- Configure hosts and resources
- Create VMs for users
- Execute cloudlets
- Observe distributed execution behavior

---

## Tools and Requirements

| Component | Version |
|--------|---------|
| Java | JDK 8+ |
| CloudSim | 3.0.3 |
| IDE | IntelliJ / Eclipse |
| OS | Windows / Linux |

---

## Datacenter Configuration

| Parameter | Value |
|----------|------|
| Datacenters | 2 |
| Hosts per DC | 1 |
| RAM | 2048 MB |
| Storage | 1,000,000 MB |
| Bandwidth | 10000 Mbps |
| Scheduler | Time Shared |

---

## VM Configuration

| VM | MIPS | RAM |
|----|------|-----|
| VM1 | 500 | 512 MB |
| VM2 | 1000 | 512 MB |

---

## Cloudlet Configuration

| Cloudlet | Length |
|----------|--------|
| C1 | 20000 |
| C2 | 40000 |
| C3 | 60000 |
| C4 | 80000 |

---

## Algorithm
1. Initialize CloudSim library  
2. Create two datacenters  
3. Create two brokers (users)  
4. Create VMs for each user  
5. Submit VMs to brokers  
6. Create cloudlets for each user  
7. Submit cloudlets to brokers  
8. Start simulation  
9. Collect results  
10. Stop simulation

 ## Sample Output

========== RESULT ==========
<img width="943" height="168" alt="image" src="https://github.com/user-attachments/assets/59d39c8b-4889-4e45-87ba-006a8c30805f" />


## Program 

package org.cloudbus.cloudsim.examples;// Source code is decompiled from a .class file using FernFlower decompiler (from Intellij IDEA).
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Calendar;
import java.util.Iterator;
import java.util.LinkedList;
import java.util.List;
import org.cloudbus.cloudsim.Cloudlet;
import org.cloudbus.cloudsim.CloudletSchedulerTimeShared;
import org.cloudbus.cloudsim.Datacenter;
import org.cloudbus.cloudsim.DatacenterBroker;
import org.cloudbus.cloudsim.DatacenterCharacteristics;
import org.cloudbus.cloudsim.Host;
import org.cloudbus.cloudsim.Pe;
import org.cloudbus.cloudsim.UtilizationModel;
import org.cloudbus.cloudsim.UtilizationModelFull;
import org.cloudbus.cloudsim.Vm;
import org.cloudbus.cloudsim.VmAllocationPolicySimple;
import org.cloudbus.cloudsim.VmSchedulerTimeShared;
import org.cloudbus.cloudsim.core.CloudSim;
import org.cloudbus.cloudsim.provisioners.BwProvisionerSimple;
import org.cloudbus.cloudsim.provisioners.PeProvisionerSimple;
import org.cloudbus.cloudsim.provisioners.RamProvisionerSimple;


public class CloudSimExample4 {
   public TwoUserSimulation() {
   }

   static Datacenter createDatacenter(String name) {
      List<Host> hostList = new ArrayList();
      List<Pe> peList = new ArrayList();
      peList.add(new Pe(0, new PeProvisionerSimple(1000.0)));
      Host host = new Host(0, new RamProvisionerSimple(2048), new BwProvisionerSimple(10000L), 1000000L, peList, new VmSchedulerTimeShared(peList));
      hostList.add(host);
      DatacenterCharacteristics characteristics = new DatacenterCharacteristics("x86", "Linux", "Xen", hostList, 10.0, 3.0, 0.05, 0.001, 0.0);

      try {
         return new Datacenter(name, characteristics, new VmAllocationPolicySimple(hostList), new LinkedList(), 0.0);
      } catch (Exception var6) {
         var6.printStackTrace();
         return null;
      }
   }

   public static void main(String[] args) {
      try {
         CloudSim.init(2, Calendar.getInstance(), false);
         Datacenter dc1 = createDatacenter("Datacenter_1");
         Datacenter dc2 = createDatacenter("Datacenter_2");
         DatacenterBroker broker1 = new DatacenterBroker("User1");
         int id1 = broker1.getId();
         Vm vm1 = new Vm(0, id1, 500.0, 1, 512, 1000L, 10000L, "Xen", new CloudletSchedulerTimeShared());
         broker1.submitVmList(Arrays.asList(vm1));
         UtilizationModel model = new UtilizationModelFull();
         Cloudlet c1 = new Cloudlet(0, 20000L, 1, 300L, 300L, model, model, model);
         Cloudlet c2 = new Cloudlet(1, 40000L, 1, 300L, 300L, model, model, model);
         c1.setUserId(id1);
         c2.setUserId(id1);
         broker1.submitCloudletList(Arrays.asList(c1, c2));
         DatacenterBroker broker2 = new DatacenterBroker("User2");
         int id2 = broker2.getId();
         Vm vm2 = new Vm(1, id2, 500.0, 1, 512, 1000L, 10000L, "Xen", new CloudletSchedulerTimeShared());
         broker2.submitVmList(Arrays.asList(vm2));
         Cloudlet c3 = new Cloudlet(2, 30000L, 1, 300L, 300L, model, model, model);
         Cloudlet c4 = new Cloudlet(3, 60000L, 1, 300L, 300L, model, model, model);
         c3.setUserId(id2);
         c4.setUserId(id2);
         broker2.submitCloudletList(Arrays.asList(c3, c4));
         CloudSim.startSimulation();
         List<Cloudlet> r1 = broker1.getCloudletReceivedList();
         List<Cloudlet> r2 = broker2.getCloudletReceivedList();
         CloudSim.stopSimulation();
         System.out.println("\nUSER 1 RESULTS:");
         Iterator var16 = r1.iterator();

         Cloudlet cl;
         while(var16.hasNext()) {
            cl = (Cloudlet)var16.next();
            System.out.println("Cloudlet " + cl.getCloudletId() + " Time " + cl.getActualCPUTime());
         }

         System.out.println("\nUSER 2 RESULTS:");
         var16 = r2.iterator();

         while(var16.hasNext()) {
            cl = (Cloudlet)var16.next();
            System.out.println("Cloudlet " + cl.getCloudletId() + " Time " + cl.getActualCPUTime());
         }
      } catch (Exception var18) {
         var18.printStackTrace();
      }

   }
}











## Result
Cloudlets from different users were successfully executed across two datacenters. The simulation demonstrates distributed task execution and broker-based scheduling.

---

## Conclusion
This experiment verifies that CloudSim accurately simulates multi-user and multi-datacenter environments. It demonstrates how distributed infrastructure handles workloads and how resources are allocated efficiently in cloud systems.

---
 
 
