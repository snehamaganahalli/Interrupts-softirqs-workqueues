**Example Work Queue**

**Implementation:**
```
struct work_struct rtm_neigh_event_work;    /* Work queue */
struct workqueue_struct *vxlan_rtm_neigh_event_wq;


INIT_WORK(&rtm_neigh_event_work, vxlan_rtm_event_handler);
queue_work(vxlan_rtm_neigh_event_wq, &rtm_neigh_event_work);


static void vxlan_rtm_event_handler()
{
}

destroy_workqueue(vxlan_rtm_neigh_event_wq);
```

**Example scenario:**

Below is a work queue

global_rtm_event_list               NEW_NEIGH|DEL_NEIGH|NEW_NEIGH|NEW_NEIGH|DEL_NEIGH

```
vxlan_tunnel_fdb_event(){​

	RTM_NEWNEIGH:​
	RTM_DELNEIGH:​
		spin_lock_bh()​
			if(global_rtm_event_list  == empty()) {​
				Insert(RTM_NEWNEIGH/ RTM_DELNEIGH, global_rtm_event_list)​
				spin_unlock_bh()​
				Work_queue_reap()​
			} else​
			 Insert(RTM_NEWNEIGH/ RTM_DELNEIGH, global_rtm_event_list)​
		spin_unlock_bh()​
}​

Work_queue_reap(){​
	while(global_rtm_event_list != empty()) {​
		spin_lock_bh()​
			element = get_element_from(global_rtm_event_list)​
		spin_unlock_bh()​

		if (element == RTM_NEWNEIGH)​
			rtm_newneigh_handler()​

		if (element == RTM_DELNEIGH)​
			rtm_delneigh_handler()​

		spin_lock_bh()​
			Del(global_rtm_event_list, element)​
		spin_unlock_bh()​
	}​
}
```
