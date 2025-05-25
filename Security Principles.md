# Security Principles
![image](https://github.com/user-attachments/assets/9343db91-d404-4dfa-8f02-adf6b2d76afd)

## The objective of this room is to:

    Explain the security functions: Confidentiality, Integrity and Availability (CIA).
    Present the opposite of the security triad, CIA: Disclosure, Alteration, and Destruction/Denial (DAD).
    Introduce the fundamental concepts of security models, such as the Bell-LaPadula model.
    Explain security principles such as Defence-in-Depth, Zero Trust, and Trust but Verify.
    Introduce ISO/IEC 19249.
    Explain the difference between Vulnerability, Threat, and Risk.

# CIA Triad
- Confidentiality ensures that only the intended persons or recipients can access the data.
- Integrity aims to ensure that the data cannot be altered; moreover, we can detect any alteration if it occurs.
- Availability aims to ensure that the system or service is available when needed.

# DAD triad
- Disclosure is the opposite of confidentiality. In other words, disclosure of confidential data would be an attack on confidentiality.
- Alteration is the opposite of Integrity. For example, the integrity of a cheque is indispensable.
- Destruction/Denial is the opposite of Availability.

# Fundamental Concepts of Security Models
## Bell-LaPadula Model
- Simple Security Property : This property is referred to as “no read up”; it states that a subject at a lower security level cannot read an object at a higher security level. This rule prevents access to sensitive information above the authorized level.
- Star Security Property : This property is referred to as “no write down”; it states that a subject at a higher security level cannot write to an object at a lower security level. This rule prevents the disclosure of sensitive information to a subject of lower security level.
- Discretionary-Security Property : This property uses an access matrix to allow read and write operations. An example access matrix is shown in the table below and used in conjunction with the first two properties.

