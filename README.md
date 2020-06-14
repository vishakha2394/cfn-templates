# cfn-templates



## pseudo-parameters

## Conditions and conditional resource creation

```yaml
AWSTemplateFormatVersion: 2010-09-09

Conditions:
  createRoleEast1: !Equals [!Ref "AWS::Region", "us-east-1"]
  createRoleEast2: !Equals [!Ref "AWS::Region", "us-east-2"]

Resources:
  RootRoleEast11:
    Condition: createRoleEast1
    Type: 'AWS::IAM::Role'
    Properties:
      RoleName: !Sub test-role-${AWS::Region}
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Principal:
              Service:
              - ec2.amazonaws.com
            Action:
              - 'sts:AssumeRole'
      Path: /
      Policies:
        - PolicyName: root
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action: '*'
                Resource: '*'
  RootInstanceProfile1:
    Condition: createRoleEast1
    Type: 'AWS::IAM::InstanceProfile'
    Properties:
      InstanceProfileName: !Sub test-profile-${AWS::Region}
      Path: /
      Roles:
        - !Ref RootRoleEast11

  RootRoleEast2:
    Condition: createRoleEast2
    Type: 'AWS::IAM::Role'
    Properties:
      RoleName: !Sub test-role-${AWS::Region}
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Principal:
              Service:
              - ecs.amazonaws.com
            Action:
              - 'sts:AssumeRole'
      Path: /
      Policies:
        - PolicyName: root
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action: '*'
                Resource: '*'
```

## CloudFormation APIs

## Change Sets