# SPARK 

## Installation

🚩 Install Java (openjdk@11 or openjdk@8 for SPARK)

    brew install openjdk@11

🚩 Add JAVA Version to ~/.zshrc or ~/.bashrc and save it using source ~/.zshrc or ~/.bashrc

    export JAVA_HOME=/opt/homebrew/opt/openjdk@11

🚩 Install Apache spark

    brew install apache-spark
    
🚩 Add SPARK_HOME variable to zshrc

    export SPARK_HOME=/opt/homebrew/opt/apache-spark/libexec/bin

🚩 Start Spark

    spark-shell/pyspark

🚩 For openjdk versions > 11, use this to run spark-shell or pyspark

    spark-shell --conf spark.driver.extraJavaOptions="-Djava.security.manager=allow"
    pyspark --conf spark.driver.extraJavaOptions="-Djava.security.manager=allow"









