using System;
using System.Collections.Generic;
using System.Runtime.Intrinsics.Arm;

namespace Scrum
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello, World!");
        }
    }
    public class ScrumProject
    {
        IList<ReleasePlan> _releasePlans;
        public ScrumProject(List<ReleasePlan>  releasePlans, string name)
        {
             _releasePlans =  releasePlans;
        }
        public void AddReleasePlan(ReleasePlan r)
        {
             _releasePlans.Add(r);
        }
        public void RemoveReleasePlan(ReleasePlan r)
        {
            _releasePlans.Remove(r);
        }
        public bool IsItDone()
        {
            return _releasePlans.Any(x => x.IsItDone());
        }
    }
    public class ReleasePlan
    {
        public ReleasePlan(IList<Sprint> sprint, TimeSpan releaseDate)
        {
            _releaseDate = releaseDate;
            _sprints = sprint;
        }
        TimeSpan _releaseDate;
        IList<Sprint> _sprints;
        public void MarkHighestPrioritySprintDone()
        {
            _sprints.OrderByDescending(item => item.Priority()).First().MarkItDone();
        }
        public void AddSprint(Sprint s)
        {
            _sprints.Add(s);
        }
        public  void RemoveSprint(Sprint s)
        {
            _sprints.Remove(s);
        }
        public bool IsItDone()
        {
            return _sprints.Any(x => x.IsItDone());
        }

    }
    public class Sprint
    {
        public Sprint(TimeSpan duration, IList<Backlog> backlogs, uint  priority )
        {
            _duration = duration;
            if (_duration.Days > 30)
                throw new ArgumentException("Too many days");
             _priority = priority;
            _backlogs = backlogs;
        }
        TimeSpan _duration;
        uint _priority;
        IList<Backlog> _backlogs;
        public  void  MarkBacklogDone(Backlog backlog)
        {
            _backlogs.First(s => s.GetIdentification() == backlog.GetIdentification()).MarkDone();
        }
        public void MarkItDone()
        {
           foreach(var b in _backlogs)
           {
               b.MarkDone();
           }
        }
        public uint Priority()
        {
            return _priority;
        }
        public void AddBackLog (Backlog b)
        {
            _backlogs.Add(b);
        }
        public  void RemoveBackLog (Backlog b)
        {
            _backlogs.Remove(b);
        }
        public  bool IsItDone ()
        {
            return _backlogs.Any(x => x.IsItDone());
        }

    }
  
    public class Backlog
    {
        private Random _r;
        public Backlog(string desc, uint priority)
        {
            _r = new Random();
            _desc = desc;
            _done = false;
            _id = _r.Next();
            _priority = priority;
        }
        public void MarkDone()
        {
            _done = true;
        }
        public int GetIdentification()
        {
            return _id;
        }
        public bool IsItDone()
        {
            return _done;
        }
        public uint Priority()
        {
            return _priority;
        }
        string _desc;
        int _id;
        uint _priority;
        bool _done;
    }
}
 