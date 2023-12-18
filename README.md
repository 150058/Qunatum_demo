using SoftwareLicenseComputing;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace LicenseComputer
{
    public class SoftwareLicenseComputer: ISoftwareLicenseComputer
    {
        #region Interface Implementation
        public int CalculateMinCopies(List<InstallationInfo> installations, int applicatoinId)
        {
            var copiesNeeded = installations
               .Where(inst => inst.ApplicationId == applicatoinId)
               .GroupBy(inst => inst.UserId)
               .Sum(group =>
               {
                   int count = group.Count();
                   return count == 1 || (count == 2 && IsMixedLaptopDesktop(group)) ? 1 : 2;
               });

            return copiesNeeded;
        }
        #endregion

        #region Private Methods
        private bool IsMixedLaptopDesktop(IEnumerable<InstallationInfo> group)
        {
            int laptopCount = group.Count(inst => inst.ComputerType.ToUpper() == "LAPTOP");
            int desktopCount = group.Count(inst => inst.ComputerType.ToUpper() == "DESKTOP");

            return (laptopCount > 0 && desktopCount > 0) || laptopCount == 2;
        } 
        #endregion
    }
}
