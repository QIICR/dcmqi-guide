# "Private" coding schemes

While DICOM allows for reuse of the codes defined in other terminologies, such as SNOMED, as well as those defined in the DICOM standard itself, so called “private” codes can also be defined by the creator of the object, when no standard codes are available. Such private codes are distinguished by a coding scheme designator that must start with the “99” prefix.

As an example, consider Dr. Smith developed a new model for estimate ADC from diffusion MRI. Since the method is new, there is no standard code for it. Dr. Smith then can establish her own coding scheme designator, say, `99DRSMITH`, and define a new code as the following triple:

`('SADC123','99SMITH','Smith Diffusion Model')`.

