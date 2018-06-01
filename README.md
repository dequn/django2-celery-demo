# django2-celery-demo
Celery integration project for Django2 demo.

## Requirements
* Docker & Docker Compose
## How to run
```
git clone https://github.com/dequn/django2-celery-demo.git
cd django2-celery-demo
# start service
docker-compose up -d

# start worker
docker-compose run web celery -A django2_celery_demo worker -l info
# start beats
docker-compose run web celery -A django2_celery_demo beat -l info --scheduler django_celery_beat.schedulers:DatabaseScheduler
```
You will see the beat logs as the following
```
[2018-06-01 17:39:38,367: INFO/MainProcess] Scheduler: Sending due task app1.tasks.hello_world_every_5_s (app1.tasks.hello_world)
[2018-06-01 17:39:43,369: INFO/MainProcess] Scheduler: Sending due task app1.tasks.hello_world_every_5_s (app1.tasks.hello_world)
[2018-06-01 17:39:48,364: INFO/MainProcess] Scheduler: Sending due task app1.app2.tasks.hello_world_every_10_s (app1.app2.tasks.hello_world)
[2018-06-01 17:39:48,371: INFO/MainProcess] Scheduler: Sending due task app1.tasks.hello_world_every_5_s (app1.tasks.hello_world)
[2018-06-01 17:39:53,373: INFO/MainProcess] Scheduler: Sending due task app1.tasks.hello_world_every_5_s (app1.tasks.hello_world)
[2018-06-01 17:39:58,366: INFO/MainProcess] Scheduler: Sending due task app1.app2.tasks.hello_world_every_10_s (app1.app2.tasks.hello_world)
```
and worker logs like this(the log time are not matched because I copied them casually).
```
[2018-06-01 17:39:08,366: INFO/MainProcess] Received task: app1.app2.tasks.hello_world[b5068437-0faa-4177-84c5-3d6c415b6eda]
[2018-06-01 17:39:08,383: INFO/ForkPoolWorker-1] Task app1.app2.tasks.hello_world[b5068437-0faa-4177-84c5-3d6c415b6eda] succeeded in 0.016141099971719086s: None
[2018-06-01 17:39:18,356: INFO/MainProcess] Received task: app1.app2.tasks.hello_world[958a2710-687f-400e-9aff-0bec12df2b46]
[2018-06-01 17:39:18,360: INFO/MainProcess] Received task: app1.tasks.hello_world[d7ecca62-87fd-484c-941c-70b5c92c1fc8]
[2018-06-01 17:39:18,385: INFO/ForkPoolWorker-2] Task app1.app2.tasks.hello_world[958a2710-687f-400e-9aff-0bec12df2b46] succeeded in 0.024647500016726553s: None
[2018-06-01 17:39:18,405: INFO/ForkPoolWorker-4] Task app1.tasks.hello_world[d7ecca62-87fd-484c-941c-70b5c92c1fc8] succeeded in 0.04313409997848794s: None
[2018-06-01 17:39:23,358: INFO/MainProcess] Received task: app1.tasks.hello_world[485d5dd3-239a-451e-a6f0-87dcd0d5a7ec]
[2018-06-01 17:39:23,386: INFO/ForkPoolWorker-3] Task app1.tasks.hello_world[485d5dd3-239a-451e-a6f0-87dcd0d5a7ec] succeeded in 0.026171699981205165s: None
[2018-06-01 17:39:28,359: INFO/MainProcess] Received task: app1.tasks.hello_world[c7c31699-2d06-41d6-a241-fba2968ef103]
[2018-06-01 17:39:28,363: INFO/MainProcess] Received task: app1.app2.tasks.hello_world[bf1c049f-5121-4a6c-99b1-69771c739b62]
[2018-06-01 17:39:28,386: INFO/ForkPoolWorker-3] Task app1.app2.tasks.hello_world[bf1c049f-5121-4a6c-99b1-69771c739b62] succeeded in 0.021645900036673993s: None
```
Two files `output.txt` and `output2.txt` will be generated in this project root, they also prove the code work and the tasks have been run.

## One Step More
You can also visit `localhost:8000/admin` to check, add, modify, delete scheduled tasks and task results(Username:root, Password:toorroot).
