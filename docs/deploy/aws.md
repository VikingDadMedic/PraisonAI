# Deploying PraisonAI Agents to AWS

This guide provides step-by-step instructions for deploying PraisonAI agents to Amazon Web Services (AWS), offering multiple deployment options to suit different requirements.

## Prerequisites

- AWS account with appropriate permissions
- AWS CLI installed and configured
- Docker installed on your local machine (for container-based deployments)
- Basic knowledge of AWS services and cloud deployment

## Deployment Options

There are several ways to deploy PraisonAI agents to AWS:
1. **AWS Lambda with API Gateway** (for serverless deployments)
2. **Amazon ECS/Fargate** (for containerized deployments)
3. **Amazon EC2** (for traditional VM-based deployments)
4. **AWS App Runner** (for simplified container deployments)

## Option 1: Deploying to EC2 (Traditional VM)

## Option 2: Deploying with Docker and ECS/Fargate

## Multi-Agent Deployment

For deploying multiple agents, you can use a single service with different endpoints:

```python
# multi-agent-api.py

from praisonaiagents import Agent

weather_agent = Agent(
 instructions="""You are a weather agent that can provide weather information for a given city.""",
 llm="gpt-4o-mini"
)

stock_agent = Agent(
 instructions="""You are a stock market agent that can provide information about stock prices and market trends.""",
 llm="gpt-4o-mini"
)

travel_agent = Agent(
 instructions="""You are a travel agent that can provide recommendations for destinations, hotels, and activities.""",
 llm="gpt-4o-mini"
)

weather_agent.launch(path="/weather", port=8080, host="0.0.0.0")
stock_agent.launch(path="/stock", port=8080, host="0.0.0.0")
travel_agent.launch(path="/travel", port=8080, host="0.0.0.0")
```

## Option 3: Serverless Deployment with AWS Lambda

For lightweight agents that don't require long-running processes, you can use AWS Lambda with API Gateway:

```python
# lambda_function.py

from praisonaiagents import Agent
import json

# Initialize the agent outside the handler for better cold start performance

agent = Agent(instructions="""You are a helpful assistant.""", llm="gpt-4o-mini")

def lambda_handler(event, context):
 try:
 # Extract the message from the event

 body = json.loads(event.get('body', '{}'))
 message = body.get('message', '')

 # Process the message with the agent

 response = agent.process(message)

 # Return the response

 return {
 'statusCode': 200,
 'headers': {
 'Content-Type': 'application/json',
 'Access-Control-Allow-Origin': '*'
 },
 'body': json.dumps({
 'response': response
 })
 }
 except Exception as e:
 return {
 'statusCode': 500,
 'headers': {
 'Content-Type': 'application/json',
 'Access-Control-Allow-Origin': '*'
 },
 'body': json.dumps({
 'error': str(e)
 })
 }
```

## Scaling and Performance

### Auto Scaling

For EC2 deployments, set up an Auto Scaling group:

```bash
# Create a launch template

aws ec2 create-launch-template \
 --launch-template-name praisonai-template \
 --version-description "Initial version" \
 --launch-template-data '{"ImageId":"ami-12345678","InstanceType":"t2.medium","UserData":"#!/bin/bash\ncd /home/ubuntu\npython3 api.py &"}'

# Create an Auto Scaling group

aws autoscaling create-auto-scaling-group \
 --auto-scaling-group-name praisonai-asg \
 --launch-template "LaunchTemplateName=praisonai-template,Version=1" \
 --min-size 1 \
 --max-size 5 \
 --desired-capacity 2 \
 --vpc-zone-identifier "subnet-12345678,subnet-87654321" \
 --target-group-arns "arn:aws:elasticloadbalancing:us-east-1:YOUR_AWS_ACCOUNT_ID:targetgroup/praisonai-tg/1234567890abcdef"
```

### Load Balancing

Set up an Application Load Balancer to distribute traffic:

```bash
# Create a load balancer

aws elbv2 create-load-balancer \
 --name praisonai-alb \
 --subnets subnet-12345678 subnet-87654321 \
 --security-groups sg-12345678

# Create a target group

aws elbv2 create-target-group \
 --name praisonai-tg \
 --protocol HTTP \
 --port 8080 \
 --vpc-id vpc-12345678 \
 --target-type instance

# Create a listener

aws elbv2 create-listener \
 --load-balancer-arn arn:aws:elasticloadbalancing:us-east-1:YOUR_AWS_ACCOUNT_ID:loadbalancer/app/praisonai-alb/1234567890abcdef \
 --protocol HTTP \
 --port 80 \
 --default-actions Type=forward,TargetGroupArn=arn:aws:elasticloadbalancing:us-east-1:YOUR_AWS_ACCOUNT_ID:targetgroup/praisonai-tg/1234567890abcdef
```

## Security Best Practices

1. **Secret Management**: Use AWS Secrets Manager for API keys
 ```bash
 # Store a secret

 aws secretsmanager create-secret \
 --name OPENAI_API_KEY \
 --secret-string "your_api_key"
 ```
2. **IAM Roles**: Use the principle of least privilege
3. **VPC**: Deploy in a private subnet with controlled access
4. **WAF**: Set up AWS WAF to protect against common web exploits
5. **API Gateway**: Implement request validation and throttling

## Monitoring and Logging

1. **CloudWatch**: Set up dashboards and alarms
 ```bash
 # Create a CloudWatch alarm for high CPU utilization

 aws cloudwatch put-metric-alarm \
 --alarm-name praisonai-high-cpu \
 --alarm-description "Alarm when CPU exceeds 80%" \
 --metric-name CPUUtilization \
 --namespace AWS/EC2 \
 --statistic Average \
 --period 300 \
 --threshold 80 \
 --comparison-operator GreaterThanThreshold \
 --dimensions Name=AutoScalingGroupName,Value=praisonai-asg \
 --evaluation-periods 2 \
 --alarm-actions arn:aws:sns:us-east-1:YOUR_AWS_ACCOUNT_ID:praisonai-alerts
 ```
2. **X-Ray**: Enable tracing for detailed request analysis
3. **CloudTrail**: Monitor API calls for security analysis
4. **Log Insights**: Query and analyze logs with CloudWatch Log Insights