---
feature name: Feature Groups
start date: 26/05/2020
rfc pr: (leave this empty)
related issue: [#164](https://github.com/aws/aws-cdk-rfcs/issues/164)
---

# Summary

> Brief description of the feature.

# Background

The CDK construct libraries are CDK modules developed and shipped by the CDK core team aimed at CDK support and
coverage of a specific AWS service. Primary modules, such as `aws-ec2`, `aws-s3` and `aws-lambda`, contain constructs
while integration pattern modules, such as, `aws-s3-notification` and `aws-lambda-event-sources` contain classes
enable service-to-service integration.

The CDK construct libraries start off in an 'Experimental' state when the libraries is being worked on and makes
it way to the final 'Generally Available' state. Learn more at [Construct Library Module
Lifecycle](https://github.com/aws/aws-cdk-rfcs/blob/master/text/0107-construct-library-module-lifecycle.md).

Maturity and stability levels of a construct library are published on the landing page for library's documentation.
See [landing page for EC2](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-ec2-readme.html) and [landing page for
APIGatewayV2](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-apigatewayv2-readme.html).

# Motivation

AWS services, in terms of its control plane surface area, come in all shapes and sizes.
On one end are services like S3, SQS and CloudTrail that come with a single resource to be configured and a handful
of properties. On the other end, are services like EC2, APIGateway, Cognito and StepFunctions that come with a
significant number of resource types and/or with a complex combination of valid properties.

Let's start with a few examples of AWS services with large surface areas, and how they are organized under the hood.

* Consider the service, Amazon Cognito. It consists of two completely separate products - user pool and identity
pool - that are packed together as a single AWS product. They have two entirely separate sets of CloudFormation
resource types that do not interact with each other.

* Consider another service, Amazon API Gateway V2. Unlike Cognito, on the surface, it has a single set of
CloudFormation resource types. but under the hood, it consists of 2 separate product variants - HTTP APIs and
Websocket APIs - where specific properties and values for these properties are only available to specific product
variants.

* Consider a third service, AWS Step Functions. Unlike the other two, this service offers only two CloudFormation
resource types but it comes with a ton of options. The service offers integrations with 10 other AWS services with
an average of 3.2 APIs per AWS service that it can integrate with.

By applying a single stability and maturity label across for each of these AWS services, a couple of problems around
'one-size-fit-all' arise.

The large surface area makes it significantly difficult to record progress and announce availability for the whole
library. If the `aws-apigatewayv2` module is announced as 'Developer Preview', it may imply to the customer that both
product variants are in that state, which may set the wrong expectation. It becomes impossible to clearly communicate
to the customer that only HTTP API is in 'Developer Preview' and not Websocket APIs.
The same can be said about the other two examples above.

A secondary problem that arises from this is one of process and tracking. The [AWS CDK
roadmap](https://github.com/aws/aws-cdk/blob/master/ROADMAP.md) today consists of tracking issues for each of the
construct libraries, and encourages customers to '+1' on the ones that they would like to see. The intent behind this
is to use this as signal for prioritization. In the services listed above with large surface area, the signal is not
granular enough to inform.

The last problem is one of internal measurements. Currently, progress on AWS service coverage via the CDK construct
libraries is measured and tasked against the number of construct libraries in a specific stability state. It becomes
hard to measure progress when working on AWS services with large surface area, and may pervade the feeling of little
or no progress. This can also create an perverse incentive to prioritize easier construct libraries first, instead of
the ones that with the impact to customers.

# Design Summary

> Summarize the approach of the feature design in a couple of sentences. Call out
> any known patterns or best practices the design is based around.

# Detailed Design

> This is the bulk of the RFC. Explain the design in enough detail for somebody
> familiar with CDK to understand, and for somebody familiar with the
> implementation to implement. This should get into specifics and corner-cases,
> and include examples of how the feature is used. Any new terminology should be
> defined here.
>
> Include any diagrams and/or visualizations that help to demonstrate the design.
> Here are some tools that we often use:
>
> - [Graphviz](http://graphviz.it/#/gallery/structs.gv)
> - [PlantText](https://www.planttext.com)

# Drawbacks

> Why should we _not_ do this? Please consider:
>
> - implementation cost, both in term of code size and complexity
> - whether the proposed feature can be implemented in user space
> - the impact on teaching people how to use CDK
> - integration of this feature with other existing and planned features
> - cost of migrating existing CDK applications (is it a breaking change?)
>
> There are tradeoffs to choosing any path. Attempt to identify them here.

# Rationale and Alternatives

> - Why is this design the best in the space of possible designs?
> - What other designs have been considered and what is the rationale for not
>   choosing them?
> - What is the impact of not doing this?

# Adoption Strategy

> If we implement this proposal, how will existing CDK developers adopt it? Is
> this a breaking change? How can we assist in adoption?

# Unresolved questions

> - What parts of the design do you expect to resolve through the RFC process
>   before this gets merged?
> - What parts of the design do you expect to resolve through the implementation
>   of this feature before stabilization?
> - What related issues do you consider out of scope for this RFC that could be
>   addressed in the future independently of the solution that comes out of this
>   RFC?

# Future Possibilities

> Think about what the natural extension and evolution of your proposal would be
> and how it would affect CDK as whole. Try to use this section as a tool to more
> fully consider all possible interactions with the project and ecosystem in your
> proposal. Also consider how this fits into the roadmap for the project.
>
> This is a good place to "dump ideas", if they are out of scope for the RFC you
> are writing but are otherwise related.
>
> If you have tried and cannot think of any future possibilities, you may simply
> state that you cannot think of anything.
