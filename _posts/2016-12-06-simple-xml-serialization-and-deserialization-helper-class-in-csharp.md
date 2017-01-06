---
layout: post
title: Simple XML serialization and deserialization helper class in C#
description: A very simple XML helper class I created for my own use in my C# projects to ease the data serialization and deserialization in open, readable dataset format.
keywords: c# programming, xml serialization, xml deserialization, xml helper class, readable dataset format
tags: [CSharp, XML, Utility]
comments: true
---

I have some projects or apps developed in .NET C# that require me to save and retrieve the data anytime, and some may need the data file available in open or readable format. So, I decided to use XML data format as I think, it is easy for user to manually modify the content of the XML file itself. To ease my work, I have created a simple helper class for XML serialization and deserialization, which I would like to share it here, so somebody can benefit from it.

### Helper class

Here is the full snippet of code or you may just download/fork it from [my public gist here](https://gist.github.com/heiswayi/cb66748bc11efe360ad6c233fa8e603f).

```csharp
using System;
using System.IO;
using System.Text;
using System.Xml;
using System.Xml.Serialization;

namespace HeiswayiNrird.Utility.Common
{
    public static class XmlHelper
    {
        /// <summary>
        /// Serialize a serializable object to XML string.
        /// </summary>
        /// <typeparam name="T">Type of object</typeparam>
        /// <param name="xmlObject">Type of object</param>
        /// <param name="useNamespaces">Use of XML namespaces</param>
        /// <returns>XML string</returns>
        public static string SerializeToXmlString<T>(T xmlObject, bool useNamespaces = true)
        {
            XmlSerializer xmlSerializer = new XmlSerializer(typeof(T));
            MemoryStream memoryStream = new MemoryStream();
            XmlTextWriter xmlTextWriter = new XmlTextWriter(memoryStream, Encoding.UTF8);
            xmlTextWriter.Formatting = Formatting.Indented;

            if (useNamespaces)
            {
                XmlSerializerNamespaces xmlSerializerNamespaces = new XmlSerializerNamespaces();
                xmlSerializerNamespaces.Add("", "");
                xmlSerializer.Serialize(xmlTextWriter, xmlObject, xmlSerializerNamespaces);
            }
            else
                xmlSerializer.Serialize(xmlTextWriter, xmlObject);

            string output = Encoding.UTF8.GetString(memoryStream.ToArray());
            string _byteOrderMarkUtf8 = Encoding.UTF8.GetString(Encoding.UTF8.GetPreamble());
            if (output.StartsWith(_byteOrderMarkUtf8))
            {
                output = output.Remove(0, _byteOrderMarkUtf8.Length);
            }

            return output;
        }

        /// <summary>
        /// Serialize a serializable object to XML string and create a XML file.
        /// </summary>
        /// <typeparam name="T">Type of object</typeparam>
        /// <param name="xmlObject">Type of object</param>
        /// <param name="filename">XML filename with .XML extension</param>
        /// <param name="useNamespaces">Use of XML namespaces</param>
        public static void SerializeToXmlFile<T>(T xmlObject, string filename, bool useNamespaces = true)
        {
            try
            {
                File.WriteAllText(filename, SerializeToXmlString<T>(xmlObject, useNamespaces));
            }
            catch
            {
                throw new Exception();
            }
        }

        /// <summary>
        /// Deserialize XML string to an object.
        /// </summary>
        /// <typeparam name="T">Type of object</typeparam>
        /// <param name="xml">XML string</param>
        /// <returns>XML-deserialized object</returns>
        public static T DeserializeFromXmlString<T>(string xml) where T : new()
        {
            T xmlObject = new T();
            XmlSerializer xmlSerializer = new XmlSerializer(typeof(T));
            StringReader stringReader = new StringReader(xml);
            xmlObject = (T)xmlSerializer.Deserialize(stringReader);
            return xmlObject;
        }

        /// <summary>
        /// Deserialize XML string from XML file to an object.
        /// </summary>
        /// <typeparam name="T">Type of object</typeparam>
        /// <param name="filename">XML filename with .XML extension</param>
        /// <returns>XML-deserialized object</returns>
        public static T DeserializeFromXmlFile<T>(string filename) where T : new()
        {
            if (!File.Exists(filename))
            {
                throw new FileNotFoundException();
            }

            return DeserializeFromXmlString<T>(File.ReadAllText(filename));
        }
    }
}
```

### Usage examples

Let's say this is your serializable object class file, e.g. `CompanyEmployee.cs`:

```csharp
using System.Collections.Generic;
using System.Xml.Serialization;

namespace XmlHelperConsoleApp
{
    [XmlRoot("MyCompanyEmployees2016")]
    public class CompanyEmployee
    {
        public List<Employee> EmployeeList { get; set; }
    }

    public class Employee
    {
        [XmlAttribute("Name")]
        public string Name { get; set; }

        [XmlAttribute("Age")]
        public int Age { get; set; }

        [XmlAttribute("Position")]
        public string Position { get; set; }

        [XmlAttribute("Department")]
        public Department Department { get; set; }
    }

    public enum Department
    {
        Technical,
        Marketing,
        Support
    }
}
```

#### Serialize object to XML string

Let's see this example code for serializing the object to XML string:

```csharp
using HeiswayiNrird.Utility.Common;
using System;
using System.Collections.Generic;
using System.Linq;

namespace XmlHelperConsoleApp
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            var employee1 = new Employee()
            {
                Name = "John Cornor",
                Age = 40,
                Position = "Technical Engineer",
                Department = Department.Technical
            };
            var employee2 = new Employee()
            {
                Name = "James Bond",
                Age = 44,
                Position = "Marketing Manager",
                Department = Department.Marketing
            };
            var employee3 = new Employee()
            {
                Name = "Jason Bourne",
                Age = 40,
                Position = "Field Application Engineer",
                Department = Department.Technical
            };

            var employeeList = new List<Employee>() { employee1, employee2, employee3 };

            var companyEmployee = new CompanyEmployee()
            {
                EmployeeList = employeeList
            };

            string xml = XmlHelper.SerializeToXmlString(companyEmployee);
            Console.WriteLine(xml);

            Console.ReadKey();
        }
    }
}
```

The code above will output XML-formatted string like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<MyCompanyEmployees2016>
  <EmployeeList>
    <Employee Name="John Cornor" Age="40" Position="Technical Engineer" Department="Technical" />
    <Employee Name="James Bond" Age="44" Position="Marketing Manager" Department="Marketing" />
    <Employee Name="Jason Bourne" Age="40" Position="Field Application Engineer" Department="Technical" />
  </EmployeeList>
</MyCompanyEmployees2016>
```

#### Deserialize XML string to object

To get for specific value from the XML string, we need to deserialize it and retrieve our desired value like this example code:

```csharp
using HeiswayiNrird.Utility.Common;
using System;
using System.Collections.Generic;
using System.Linq;

namespace XmlHelperConsoleApp
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            // string xml = <...XML-formatted string...>
            var obj = XmlHelper.DeserializeFromXmlString<CompanyEmployee>(xml);

            // Get list of employee names from Technical department
            var getTechnicalDept = obj.EmployeeList.Where(x => x.Department == Department.Technical).ToList();

            foreach (var employee in getTechnicalDept)
                Console.WriteLine(employee.Name);

            Console.ReadKey();
        }
    }
}
```

The code above will output something like this:

```
John Cornor
Jason Bourne
```

Yep, as simple as that!

### Credits

Previously, I was facing with one issue with my XML helper class when I was trying to deserialize the XML string with this kind of exception message:

```
Data at the root level is invalid. Line 1, position 1.
```

After I googled for an answer, I found [this blog article](http://www.ipreferjim.com/2014/09/data-at-the-root-level-is-invalid-line-1-position-1/) which found the solution for the same issue I was facing with. It's known as **byte order mark (BOM)**.

So, I fixed it into my XML helper class.
