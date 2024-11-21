**Example Work Queue**

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
