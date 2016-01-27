# ReflectionProject
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Reflection;
using System.Collections;

namespace ILLYDevTask
{
    public class Program
    {
        private static void Main(string[] args)
        {
            // ClassA[] objects = { new ClassA("Crage", 2500, new DateTime(2001, 10, 20)),
            //                   new ClassA("Martin", 2501, new DateTime(2001,10,20)), };

            ClassA classA = new ClassA("Crage", 2500, new DateTime(2001, 10, 20),new List<Department>{new Department("HR","UK")});
            ClassA classA1 = new ClassA("Martin", 2501, new DateTime(2001, 10, 20), new List<Department>{new Department("Audit","USA")});


            /* for (int i = 0; i < objects.Length - 1; i++)
            {
                Console.WriteLine("Comparing {0} and {1}:",i,i+1);
                foreach (PropertyCompareResult resultItem in Compare(objects[i], objects[i + 1]))
                {*/
            List<PropertyCompareResult> resultItems = Compare(classA, classA1);
            foreach (PropertyCompareResult resultItem in resultItems)
            {
                Console.WriteLine(
                "  Property name: {0} -- old: {1}, new: {2}", resultItem.Name, resultItem.NewValue, resultItem.OldValue);
            }
           // Console.WriteLine(
           //     "  Property name: {0} -- old: {1}, new: {2}", resultItem.Name, resultItem.NewValue, resultItem.OldValue);
            // }
        }



        public static List<PropertyCompareResult> Compare<T>(T oldObject, T newObject)
        {
           
            PropertyInfo[] properties = typeof(T).GetProperties();

           
            List<PropertyCompareResult> results = new List<PropertyCompareResult>();

            foreach (PropertyInfo propertyInfo in properties)
            {
                object oldValue = propertyInfo.GetValue(oldObject), newValue = propertyInfo.GetValue(newObject);
                 if (oldValue.GetType().IsGenericType)
                 {
                     Type oldproperty = oldValue.GetType().GetGenericArguments()[0];
                     PropertyInfo[] property = oldproperty.GetProperties();
                     //foreach (PropertyInfo info in property)
                     //{
                      //   object oValue = info.GetValue(oldValue);
                    // }

                 }
                if (!object.Equals(oldValue, newValue))
                {
                    results.Add(new PropertyCompareResult(propertyInfo.Name, oldValue, newValue));
                }
            }
            return results;

        }
    }
}
