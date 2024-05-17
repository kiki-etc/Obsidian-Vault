## Background
<br>The Industrial Revolution phases mark significant improvements in technology and productivity.

(1784) First Industrial Revolution - invention of the steam engine, simple mechanization
(1870) Second Industrial Revolution - electricity, mass production, assembly lines
(1969) Third Industrial Revolution - silicon-based microprocessors, computers & electronics, The Automated Factory
(Today) Fourth Industrial Revolution - cyber physical systems, IoT, The Intelligent Factory

>[!note]- The 4IR Components
>1. Development of faster computing hardware
>2. Availability of high speed communication infrastructure
>3. Development of bolder algorithms
>4. Big data and available cloud services
>5. Availability of specialised arrays of processors

## Overview
<br>What is artificial intelligence?
Focused on designing computational systems that can interpret sensory input, learn from experience, understand human language, plan ahead, and support intelligent decision-making.

Models + Data + Rules = what/when, why, how \[prescriptive analytics]

General Programming uses data and rules to generate a desired output, compared to Machine Learning Models which take data and output to learn rules.

### Data Science vs AI vs ML
<br>Data Science is based on strict analytical evidence. It deals with structured and unstructured data and includes various data operations.
Artificial Intelligence imparts human intelligence into machines. It uses logic and decision trees. It includes machine learning.
Machine Learning is a subset of AI. It uses statistical models. Machines improve with experience.
### Decision Making Process (Analytics Maturity)
<br>The decision-making process in data analytics involves identifying a problem or opportunity, collecting relevant data, preprocessing and exploring the data to understand its patterns, formulating hypotheses, selecting and training appropriate models, evaluating their performance, and making decisions based on the insights gained. These decisions are then implemented into the business workflow, with continuous monitoring and iteration to ensure effectiveness and alignment with evolving objectives.
#### Descriptive Analytics
This type of analytics focuses on summarising historical data to describe what happened in the past. It provides insights into trends, patterns, and relationships within the data. Descriptive analytics answers questions like "What happened?" and "What is the current state?"
#### Diagnostic Analytics
Diagnostic analytics delves deeper into the data to understand why certain events happened. It aims to identify the root causes of past outcomes or events. Diagnostic analytics answers questions like "Why did it happen?" and "What were the factors contributing to the outcome?"
#### Discovery Analytics
Discovery analytics involves the exploration of data to uncover hidden patterns, correlations, or insights that were previously unknown. It often involves techniques such as data mining, clustering, and anomaly detection. Discovery analytics aims to reveal new opportunities or areas for improvement.
#### Predictive Analytics
Predictive analytics uses historical data and statistical algorithms to forecast future outcomes or trends. It analyses patterns in the data to make predictions about what is likely to happen. Predictive analytics answers questions like "What is likely to happen next?" and "What are the future trends?"
#### Prescriptive Analytics
Prescriptive analytics goes beyond predicting future outcomes to recommend actions to optimize or improve those outcomes. It considers different courses of action and their potential impact, helping decision-makers choose the best course of action. Prescriptive analytics answers questions like "What should we do?" and "What actions will lead to the best outcomes?"

- **Analytics Maturity**: As organizations progress in their analytics journey, they typically evolve from basic descriptive analytics to more advanced forms such as diagnostic, predictive, and prescriptive analytics. This progression reflects increasing sophistication in data analysis capabilities, technology adoption, and organizational culture.
- **Value**: The value derived from analytics increases as organizations move up the maturity curve. While descriptive analytics provides valuable insights into past performance, diagnostic analytics helps identify areas for improvement. Predictive and prescriptive analytics enable proactive decision-making and optimization, leading to more significant business impact and value creation.
- **Reporting Approaches**: Adhoc reporting typically aligns with descriptive analytics, providing on-demand reports and dashboards to monitor past performance. As organizations mature in analytics, they transition to more optimized reporting approaches that leverage advanced analytics techniques to provide deeper insights, forecasts, and recommendations. Optimized reporting integrates diagnostic, predictive, and prescriptive analytics to support strategic decision-making and drive business outcomes.
### Big Data
<br>Big data refers to large and complex datasets that are difficult to process using traditional data processing applications.
It is characterized by several qualities commonly referred to as the "Vs":

1. **Volume**: Volume refers to the sheer size of data generated, collected, and stored. Big data involves datasets that are too large to be processed using conventional methods. This includes terabytes, petabytes, or even exabytes of data.
    
2. **Variety**: Variety refers to the diverse types of data that are generated and collected. Big data encompasses structured data (e.g., databases, spreadsheets), semi-structured data (e.g., XML, JSON), and unstructured data (e.g., text, images, videos). Managing and analyzing these various data types present challenges due to their disparate formats.
    
3. **Velocity**: Velocity relates to the speed at which data is generated, collected, and processed. With the advent of technologies such as IoT devices, social media, and sensors, data is produced at an unprecedented rate. Real-time or near-real-time processing is often necessary to derive actionable insights from rapidly changing data streams.
    
4. **Veracity**: Veracity refers to the quality or reliability of the data. Big data often includes data from diverse sources with varying levels of accuracy, consistency, and trustworthiness. Ensuring data veracity involves addressing issues such as data errors, inconsistencies, biases, and uncertainties.
    
5. **Variability**: Variability refers to the inconsistency or fluctuation in the data over time. Big data may exhibit temporal variations, seasonal patterns, or irregular fluctuations, making it challenging to analyze and interpret. Analytical techniques must account for such variability to extract meaningful insights.
    
6. **Value**: Value refers to the potential insights, knowledge, or benefits that can be derived from analyzing big data. Despite the challenges posed by volume, variety, velocity, and other characteristics, big data analytics offers opportunities to uncover valuable insights, improve decision-making, drive innovation, and create business value.
    
7. **Volatility**: Volatility refers to the dynamic nature of data, including changes in data sources, formats, and requirements over time. Big data environments are often characterized by evolving technologies, changing business needs, and shifting regulatory requirements, necessitating adaptable and agile data management and analytics strategies.
    
8. **Visualization**: Visualization refers to the process of representing big data visually, typically through charts, graphs, maps, and dashboards. Visualization facilitates the exploration, analysis, and communication of complex datasets, enabling stakeholders to gain insights and make data-driven decisions more effectively.
#### Big Data Ecosystem
<br>Building a big data ecosystem on commodity hardware involves several steps, from collecting data from various sources to analyzing and visualizing it for insights.

1. **Data Sources**:
   - Big data ecosystems typically begin with identifying and accessing relevant data sources. These sources may include structured data from databases, semi-structured data from sources like APIs and log files, and unstructured data from sources like social media, sensor data, and text documents.
   - Data sources can vary widely depending on the organization's industry, operations, and objectives.

2. **Data Acquisition**:
   - Once data sources are identified, the next step is to acquire and ingest data into the big data ecosystem. This involves collecting data from various sources and transferring it to a centralized storage system for processing.
   - Data acquisition methods may include batch processing, where data is collected periodically in large batches, or real-time/streaming processing, where data is ingested continuously as it is generated.

3. **Data Storage**:
   - After data acquisition, the data needs to be stored in a scalable and cost-effective manner. Commodity hardware, consisting of off-the-shelf servers and storage devices, is often used to build the storage layer of the big data ecosystem.

4. **Data Analysis**:
   - With data stored in the big data ecosystem, the next step is to analyze it to derive meaningful insights. This involves processing and transforming raw data into actionable information using various analytical techniques and algorithms.
   - Advanced analytics techniques such as machine learning, predictive modeling, and natural language processing may be applied to uncover patterns, trends, and correlations in the data.

5. **Reporting and Visualization**:
   - Once data analysis is complete, the insights obtained need to be communicated effectively to stakeholders. Reporting and visualization tools are used to create dashboards, reports, and interactive visualizations that convey key findings and metrics.
   - Visualizations such as charts, graphs, maps, and heat maps help stakeholders understand complex data patterns and make data-driven decisions.
***<br>Challenges with Big Data

1. **Data Storage Challenges**:

   - **Scalability**: As the volume of data grows exponentially, traditional storage solutions may struggle to scale accordingly. This necessitates scalable storage architectures capable of accommodating large datasets.
   
   - **Cost**: Storing massive amounts of data can be expensive, especially when using high-performance storage systems. Organizations must balance performance requirements with cost considerations to optimize storage expenditure.
   
   - **Data Retention Policies**: Organizations must adhere to data retention policies and regulatory requirements, which may dictate how long data must be stored and in what format. This adds complexity to data storage management.
   
   - **Data Security**: Ensuring the security and integrity of stored data is crucial, particularly when dealing with sensitive or confidential information.

2. **Memory Challenges (associated with RAM)**:

   - **Memory Limitations**: In-memory processing offers significant performance advantages but is limited by the amount of available memory. Large datasets may exceed the capacity of available memory, necessitating disk-based processing or distributed computing frameworks.
   
   - **Memory Management**: Efficient memory management is critical to optimize performance and avoid resource contention. Techniques such as data compression, caching, and partitioning can help mitigate memory-related challenges.
   
   - **Data Movement**: Moving large volumes of data between memory and storage can incur significant overhead, impacting performance.

3. **Computational Power Challenges**:

   - **Processing Speed**: Analyzing and processing large datasets require substantial computational power. High-performance computing (HPC) systems and parallel processing techniques are often employed to accelerate data processing tasks.
   
   - **Complexity of Algorithms**: Some data analysis tasks involve computationally intensive algorithms that may require significant processing power. Implementing and optimizing these algorithms for distributed or parallel execution can be challenging.
   
   - **Energy Consumption**: High computational power often translates to increased energy consumption, leading to environmental concerns and operational costs. Energy-efficient computing techniques and hardware optimizations are increasingly important for sustainable data processing.