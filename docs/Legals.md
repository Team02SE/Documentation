# Legal and Licensing Considerations for the SKC Government Project

## Project Context and Legal Sensitivity

This project is developed for SKC in the Netherlands and involves the analysis of documents related to various forms of criminal activity. Because the client operates in a government context and the project is covered by a Non Disclosure Agreement, legal compliance and licensing awareness are critical from the earliest design phase. All technical decisions must respect confidentiality, data protection obligations, and restrictions on how tools, libraries, and services are used, deployed, and shared.

## Non Disclosure Agreements and Confidentiality

The NDA governing this project imposes strict limitations on the disclosure of information related to data, system design, and implementation details. Any software components used in the project must not introduce obligations that conflict with these confidentiality requirements. Licenses that require public disclosure of source code, internal architecture, or derived works can pose legal risks in an NDA protected government environment. Team members are required to limit access to project materials, avoid external code sharing, and ensure that documentation and repositories are secured according to SKC policies.

## Software Licensing Considerations

Selecting third party libraries requires careful review of their licenses to ensure compatibility with government use and NDA constraints. Permissive licenses such as MIT, BSD, and Apache 2.0 are generally suitable because they allow modification and internal use without requiring source code disclosure. These licenses typically only require attribution and preservation of copyright notices, which can be handled internally.

Copyleft licenses such as GPL and AGPL require special attention. These licenses can obligate the project to release source code of derivative works, especially when software is distributed or accessed over a network. In a government and law enforcement context, this can conflict with security requirements and confidentiality agreements. As a result, such licenses should be avoided unless explicitly approved by SKC legal and compliance teams.

Commercial and proprietary licenses may also be used, provided that SKC holds valid agreements and the license terms allow deployment on internal servers. Any restrictions related to user limits, geographic use, or data processing locations must be verified before adoption.

## Data Protection and Regulatory Compliance

Because the project processes sensitive criminal investigation data, compliance with European regulations such as the General Data Protection Regulation is essential. Tools and libraries must support secure data handling, access control, audit logging, and data minimization. Licensing terms must not grant vendors rights to access, reuse, or analyze the data. This is especially important when considering cloud based services or analytics tools.

## Use of Azure in a Government Context

Microsoft Azure has established agreements and compliance certifications that support government and public sector use, including alignment with European data protection standards. Azure provides services that meet high security and compliance requirements, which makes it a viable option for development, testing, or supporting infrastructure. However, the use of Azure services must still be evaluated against SKC internal policies and the specific terms of their government agreements.

## On Premises Deployment and Security

Despite the availability of compliant cloud platforms, SKC will deploy the final application on their own servers. This decision is driven by security, control, and data sovereignty requirements associated with criminal intelligence analysis. On premises deployment reduces exposure to external networks and ensures that sensitive data remains fully under SKC control. All selected tools and libraries must therefore support offline or internal server deployment without requiring external service dependencies or license validation through third party servers.

## Conclusion

Legal and licensing considerations play a central role in this project due to the NDA, government client, and sensitive nature of the data. By prioritizing permissive licenses, avoiding disclosure obligations, respecting data protection laws, and aligning with SKC deployment and security policies, the project can proceed into the design phase with reduced legal risk. Ongoing verification with experienced teammates and SKC stakeholders is essential to ensure continued compliance throughout development.
