# Hadoop Admin commands
- `hdfs fsch /`
- `hdfs dfsadmin -report`

# Concept : Preferred Node Location Data
We have to tell spark where the data is located, in order to lauch its containers there. This has to be done when creating spark context.
```
val locData = InputFormatInfo.computetePreferredLocations(
	Seq(
		new InputFormatInfo(conf,
						 classOf[TextInputFormat],
						  new Path("myfile.txt")
						  )
		)
	)
val sc = new SparkContext(conf, locData)
```
# Methods for accessing spark via jupyter
- Apache Toree
- Jupyter Enterprise Gateway -> not preferred (used in large enterprises with alot of devlopers)
- livy -> not preferred (A spark rest server)
- zeppelin -> (not preferred )separate project, which provides web based notebooks.
- pyspark
- sparkmagic -> (not preferred )works with livy. 